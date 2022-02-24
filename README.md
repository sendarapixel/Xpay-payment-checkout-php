<?php  


    define('host', 'production');


    if(host == 'development'){
       $HOST = 'test.xpay.cash';
    }elseif(host == 'production'){
       $HOST = 'xpay.cash';
    }

   // $email = "alexanderconmic@gmail.com";
    $token = "8de64370e87ee4f844dc0e88f7a8783db28edafa";
    $headers = array(
    'Authorization: Token 8de64370e87ee4f844dc0e88f7a8783db28edafa'
    );



    $ch = curl_init();

    /*$payload = json_encode(array(
    'token' => $token));*/
    //$payload = json_encode($datas);

    curl_setopt($ch, CURLOPT_URL, "https://".$HOST."/api/v1/users/");
    //curl_setopt($ch, CURLOPT_HEADER, 1);
    curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
    //return response instead of outputting
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    $data = curl_exec($ch);


   // echo var_dump($data);




    curl_close($ch);









    /* 

//////////// CONSIGUE LA INFORMACION DE TU CUENTA //////////////////


<?php

$url = "https://xpay.cash/api/v1/users/";

$curl = curl_init($url);
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);

$headers = array(
   "Authorization: Token 8de64370e87ee4f844dc0e88f7a8783db28edafa",
);
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
//for debug only!
curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, false);
curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);

$resp = curl_exec($curl);
curl_close($curl);
var_dump($resp);

?>


////////////////////VERIFICA TUS MONEDAS DISPONIBLES////////////
$url = "https://xpay.cash/api/v1/transactions/available/currencies/9000.00/VES/";

$curl = curl_init($url);
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);

$headers = array(
   "Authorization: Token 8de64370e87ee4f844dc0e88f7a8783db28edafa",
);
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
//for debug only!
curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, false);
curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);

$resp = curl_exec($curl);
curl_close($curl);
var_dump($resp);




<?php
//////////////////CREAR FACTURA //////////////
$url = "https://xpay.cash/api/v1/transactions/create/";

$curl = curl_init($url);
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_POST, true);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);

$headers = array(
   "Content-Type: application/json",
   "cache-control: no-cache",
   "Authorization: Token 8de64370e87ee4f844dc0e88f7a8783db28edafa",
);
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);

$data = <<<DATA
{
        "src_currency": "BTC",
        "amount": 1,
        "exchange_id": 13,
        "tgt_currency": "VES",
        "callback": "https://localhost"
      }
DATA;

curl_setopt($curl, CURLOPT_POSTFIELDS, $data);

///////////////////////////////PASO 3//////////////////////
verificar las transacciones
$url = "https://xpay.cash/api/v1/transactions/396844/";

$curl = curl_init($url);
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_POST, true);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);

$headers = array(
   "Content-Type: application/json",
   "cache-control: no-cache",
   "Authorization: Token 8de64370e87ee4f844dc0e88f7a8783db28edafa",
);
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);



$data = <<<DATA
{
        "src_currency": "BTC",
        "amount": 1,
        "exchange_id": 13,
        "tgt_currency": "VES"
      }
DATA;

curl_setopt($curl, CURLOPT_POSTFIELDS, $data);

?>
*/





<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bitcoin store</title>
    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
    <!-- Custom CSS -->
    <link rel="stylesheet" href="css/style.css">
</head>
<body>

    <!-- Navigation bar -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="row">
            <a class="navbar-brand" href="index.php">Bitcoin Example</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav mr-auto">
                <li class="nav-item active">
                    <a class="nav-link" href="index.php">Store <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item active">
                    <a class="nav-link" href="orders.php">Purchases</a>
                </li>
                
                </ul>
                
            </div>
        </div>
    </nav>

    <!-- Invoice -->
    
    <main>
        <div class="row">
            <h1 style="width:100%;">Invoice</h1>
            <p style="display:block;width:100%;">Please pay <?php// echo round(USDtoBTC($price), 8); ?> BTC to address: <span id="address"><?php// echo $address; ?></span></p>
            <?php
            // QR code generation using google apis
            $cht = "qr";
            $chs = "300x300";
            $chl = $address;
            $choe = "UTF-8";

            $qrcode = 'https://chart.googleapis.com/chart?cht=' . $cht . '&chs=' . $chs . '&chl=' . $chl . '&choe=' . $choe;
            ?>
            <div class="qr-hold">
                <img src="<?php echo $qrcode ?>" alt="My QR code" style="width:250px;">
            </div>
            
            
            <p style="display:block;width:100%;">Status: <?php //echo $status; ?></p>
            <?php// echo $info; ?>
            <div id="info"></div>
            <h2 style="width:100%;margin-top: 20px;">What you're paying for:</h2>
            <h4 style="width:100%;margin-top: 20px;"><?php// echo getProduct($product); ?></h4>
            <p><?php// echo getDescription($product); ?></p>
        </div>
    </main>
    <script>
        var status = <?php echo $statusval; ?>
        
        // Create socket variables
        if(status < 2 && status != -2){
        var addr =  document.getElementById("address").innerHTML;
        var wsuri2 = "wss://www.blockonomics.co/payment/"+ addr;
        // Create socket and monitor
        var socket = new WebSocket(wsuri2, "protocolOne")
            socket.onmessage = function(event){
                console.log(event.data);
                response = JSON.parse(event.data);
                //Refresh page if payment moved up one status
                if (response.status > status)
                  setTimeout(function(){ window.location=window.location }, 1000);
            }
        }
        
    </script>
    <!-- Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js" integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV" crossorigin="anonymous"></script>
</body>
</html>