# PayPal
    PayPal payment module.
    
# PayPal payment form
     <form action="<?php echo PAYPAL_URL_TEST; ?>" method="post" name="frmPayPal1">
        <input type="hidden" name="business" value="<?php echo PAYPAL_ID; ?>">
        <input type="hidden" name="cmd" value="_xclick">
        <input type="hidden" name="item_name" value="<?php echo $product;?>">
        <input type="hidden" name="item_number" value="<?php echo $item_number;?>">
        <input type="hidden" name="credits" value="510">
        <input type="hidden" name="userid" value="1">
        <input type="hidden" name="amount" value="<?php echo $price;?>">
        <input type="hidden" name="cpp_header_image" value="img/test.jpeg">
        <input type="hidden" name="no_shipping" value="1">
        <input type="hidden" name="currency_code" value="<?php echo $currency;?>">
        <input type="hidden" name="handling" value="0">
        <input type="hidden" name="cancel_return" value="<?php echo SITE_URL; ?>/modules/paypal-payment/cancel.php"><!-- Set its value according to your configuration settings -->
        <input type="hidden" name="return" value="<?php echo SITE_URL; ?>/modules/paypal-payment/success.php"><!-- Set its value according to your configuration settings -->
        <input type="image" src="https://www.sandbox.paypal.com/en_US/i/btn/btn_buynowCC_LG.gif" border="0" name="submit" alt="PayPal - The safer, easier way to pay online!">
        <img alt="" border="0" src="https://www.sandbox.paypal.com/en_US/i/scr/pixel.gif" width="1" height="1">
    </form>
    
### Handling payment response

    $item_no            = $_REQUEST['item_number'];
    $item_transaction   = $_REQUEST['tx']; // Paypal transaction ID
    $item_price         = $_REQUEST['amt']; // Paypal received amount
    $item_currency      = $_REQUEST['cc']; // Paypal received currency type
    global $mysqli;
    $row = array();
    //Getting product details
    $stmt = $mysqli->prepare("SELECT pid,product,product_img,price,currency FROM products WHERE pid = '$item_no'");
    if($stmt && $stmt->execute()){
        $stmt->bind_result($pid,$product,$product_img,$price,$currency);
        while($stmt->fetch()){
            $row[] = array('pid'=>$pid,'product'=>$product,'product_img'=>$product_img,'price'=>$price,'currency'=>$currency);
        }
        $stmt->close();
        
        
    }
    if(empty($row)){
        echo "<h1>Payment Failed</h1>";
        exit;
    }
  
### Dependecies
        PHP 5.5
        MySql 5.6.17

### Database
    Need to create a database named rets_api and improt paypal.sql file to that db.

### Database configurations - db_config.php
    Update database credentials
    define ('DB_USER', "");
    define ('DB_PASSWORD', "");
    define ('DB_DATABASE', "paypal");
    define ('DB_HOST', "");

