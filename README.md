<!-- Homepage Hero Section -->
<section class="hero">
    <h1>Discover the Latest Fashion Trends</h1>
    <p>Shop quality clothing with exclusive discounts</p>
    <a href="shop.html" class="btn">Shop Now</a>
</section>

.hero {
    text-align: center;
    padding: 50px;
    background: url('banner.jpg') no-repeat center center/cover;
    color: white;
}
.btn {
    background-color: #ff6600;
    color: white;
    padding: 10px 20px;
    text-decoration: none;
    font-size: 18px;
    border-radius: 5px;
}
<?php
session_start();
include 'db_connect.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $email = $_POST['email'];
    $password = $_POST['password'];

    $stmt = $conn->prepare("SELECT id, password FROM users WHERE email = ?");
    $stmt->bind_param("s", $email);
    $stmt->execute();
    $stmt->store_result();
    $stmt->bind_result($userId, $hashedPassword);
    
    if ($stmt->num_rows > 0) {
        $stmt->fetch();
        if (password_verify($password, $hashedPassword)) {
            $_SESSION['user_id'] = $userId;
            header("Location: dashboard.php");
        } else {
            echo "Invalid login credentials.";
        }
    } else {
        echo "User not found.";
    }
}
?>
document.getElementById('priceFilter').addEventListener('change', function() {
    let minPrice = this.value.split('-')[0];
    let maxPrice = this.value.split('-')[1];
    
    document.querySelectorAll('.product-item').forEach(item => {
        let price = parseFloat(item.getAttribute('data-price'));
        if (price >= minPrice && price <= maxPrice) {
            item.style.display = 'block';
        } else {
            item.style.display = 'none';
        }
    });
});
session_start();
include 'db_connect.php';

$productId = $_POST['product_id'];
$quantity = $_POST['quantity'];

if (!isset($_SESSION['cart'])) {
    $_SESSION['cart'] = [];
}

if (array_key_exists($productId, $_SESSION['cart'])) {
    $_SESSION['cart'][$productId] += $quantity;
} else {
    $_SESSION['cart'][$productId] = $quantity;
}

echo json_encode(["message" => "Product added to cart!"]);
