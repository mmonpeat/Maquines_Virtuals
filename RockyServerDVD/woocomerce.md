He instal·lat Woocomerce
Vas a extensions 

![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/84a7ab9c-8ea7-40fd-80ca-1ad908641577)

![imatge](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/56d49a6c-46e3-4085-b6f3-eaf005135f41)

Per canviar el tema de la web 

![temes](https://github.com/mmonpeat/Maquines_Virtuals/assets/115364869/c9d3c0c7-6a31-4504-8199-c9c488413dc6)

## admin/css/mystyle.css
```css

table, tr, td, th {
        border: 1px solid black;
        border-collapse: collapse;
}
td, th {
        padding: 5px 10px;
        min-width: 30px;
        text-align: center;
}
table {
        margin: auto;
}
table.dataTable thead .sorting:after,
table.dataTable thead .sorting:before,
table.dataTable thead .sorting_asc:after,
table.dataTable thead .sorting_asc:before,
table.dataTable thead .sorting_asc_disabled:after,
table.dataTable thead .sorting_asc_disabled:before,
table.dataTable thead .sorting_desc:after,
table.dataTable thead .sorting_desc:before,
table.dataTable thead .sorting_desc_disabled:after,
table.dataTable thead .sorting_desc_disabled:before {
          bottom: .5em;
  }

```

## admin/js/myscript.js
```
$(document).ready(function () {
          $('#tablee').DataTable();
          $('.dataTables_length').addClass('bs-select');
});
```

## templates/admin.php
```php

<?php
$servername = "localhost";
$username = "root";
$password = "123456789";
$dbname = "wp";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);
// Check connection
if ($conn->connect_error) {
die("Connection failed: " . $conn->connect_error);
}

$sql = "SELECT * FROM wp_wc_orders";
$result = $conn->query($sql);
?>

<div class="table-wrapper">
<table id="tablee" class="table table-striped table-bordered table-sm" cellspacing="0" width="100%">
<thead>
<tr>
        <th>ID</th>
        <th>Date</th>
        <th>Status</th>
        <th>Currency</th>
        <th>Product</th>
        <th>Total</th>
</tr>
</thead>
</tbody>
<?php
while($row = $result->fetch_assoc()) {
        $id = $row["id"];
        $itemresult = $conn->query("SELECT order_item_name AS itemname FROM wp_woocommerce_order_items WHERE order_id = $id")->fetch_assoc();//Agafa info de la bbdd i la guarda en una variable en un array asosiatiu
        $itemname = $itemresult !== null ? $itemresult["itemname"] : 'NO SE';
        $dateres = $conn->query("SELECT date_created AS date FROM wp_wc_order_stats WHERE order_id = $id")->fetch_assoc();
        $date = $dateres !== null ? $dateres["date"] : 'casi';
        echo '<tr><td>'. $id . '</td><td>'. $date . '</td><td>'. $row["status"] .'</td><td>'. $row["currency"] ."</td><td>$itemname</td><td>" . $row["total_amount"] . '</td></tr>';
}
$conn->close();
?>
</tbody>
</table>
</div>
```
