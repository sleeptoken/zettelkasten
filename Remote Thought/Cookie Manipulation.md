
2024-07-12 21:13

Source: #web #overthewire #natas 

Tags: [[PHP]]
## Natas 11

```
<?php
function xor_encrypt($in) {
    $key = json_encode(array( "showpassword"=>"no", "bgcolor"=>"#ffffff"));
    $text = $in;
    $outText = '';

    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}
$cookie = "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D";
echo xor_encrypt(base64_decode($cookie))
?>
```
- above code gives the key of `xor` operation. 
- replace the `$key` in the `xor_encrypt` function with the new key and replace the echo command with the new one. 
```
echo base64_encode(xor_encrypt(json_encode(array( "showpassword"=>"yes", "bgcolor"=>"#ffffff"))))
```
we get a new cookie
to change cookie open inspect element and type the following in the console
```
document.cookie="data=new_cookie"
```
### References
https://www.youtube.com/watch?v=blNRSTBfyVA