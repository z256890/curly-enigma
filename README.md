<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>高级发卡网 - 在线购买卡密</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.0/css/bootstrap.min.css" rel="stylesheet">
    <style>
        /* 自定义样式 */
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f8f9fa;
        }

        .navbar {
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .product-card {
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        #verifyResult {
            font-size: 1.2em;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <!-- 导航栏 -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="#">高级发卡网</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <a class="nav-link active" href="#">首页</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#cart">购物车</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#verify">卡密验证</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <!-- 产品列表 -->
    <section class="container mt-5">
        <h2 class="text-center mb-4">热门产品</h2>
        <div class="row" id="productList">
            <!-- 产品动态加载 -->
        </div>
    </section>

    <!-- 购物车 -->
    <section class="container mt-5" id="cart">
        <h2 class="text-center mb-4">购物车</h2>
        <div class="table-responsive">
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th>产品名称</th>
                        <th>价格</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody id="cartItems">
                    <!-- 购物车内容动态加载 -->
                </tbody>
            </table>
        </div>
        <div class="text-end">
            <h5>总价：<span id="totalPrice">￥0.00</span></h5>
            <button class="btn btn-success" onclick="checkout()">结算</button>
        </div>
    </section>

    <!-- 卡密验证 -->
    <section class="container mt-5" id="verify">
        <h2 class="text-center mb-4">卡密验证</h2>
        <form id="verifyForm" class="text-center">
            <div class="mb-3">
                <input type="text" class="form-control" id="cardCode" placeholder="请输入卡密" required>
            </div>
            <button type="submit" class="btn btn-primary">验证</button>
        </form>
        <p id="verifyResult" class="text-center mt-3"></p>
    </section>

    <!-- 页脚 -->
    <footer class="bg-dark text-white text-center py-3 mt-5">
        <p>&copy; 2023 高级发卡网. 保留所有权利。</p>
    </footer>

    <!-- Bootstrap JS -->
    <script src="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.3.0/js/bootstrap.bundle.min.js"></script>
    <script>
        // 模拟产品数据
        const products = [
            { id: 1, name: "10元充值卡", price: 10 },
            { id: 2, name: "50元充值卡", price: 50 },
            { id: 3, name: "100元充值卡", price: 100 }
        ];

        // 购物车数据
        let cart = [];
        let totalPrice = 0;

        // 动态加载产品
        function loadProducts() {
            const productList = document.getElementById('productList');
            productList.innerHTML = products.map(product => `
                <div class="col-md-4 mb-4">
                    <div class="card product-card">
                        <div class="card-body">
                            <h5 class="card-title">${product.name}</h5>
                            <p class="card-text">价格：￥${product.price.toFixed(2)}</p>
                            <button class="btn btn-primary" onclick="addToCart(${product.id})">加入购物车</button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // 添加到购物车
        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            cart.push(product);
            totalPrice += product.price;
            updateCart();
        }

        // 更新购物车
        function updateCart() {
            const cartItems = document.getElementById('cartItems');
            cartItems.innerHTML = cart.map(item => `
                <tr>
                    <td>${item.name}</td>
                    <td>￥${item.price.toFixed(2)}</td>
                    <td><button class="btn btn-danger btn-sm" onclick="removeFromCart('${item.id}')">移除</button></td>
                </tr>
            `).join('');
            document.getElementById('totalPrice').textContent = `￥${totalPrice.toFixed(2)}`;
        }

        // 从购物车移除
        function removeFromCart(productId) {
            const index = cart.findIndex(item => item.id === productId);
            if (index !== -1) {
                totalPrice -= cart[index].price;
                cart.splice(index, 1);
                updateCart();
            }
        }

        // 结算
        function checkout() {
            if (cart.length === 0) {
                alert('购物车为空，请先添加商品！');
            } else {
                alert(`结算成功！总价：￥${totalPrice.toFixed(2)}`);
                cart = [];
                totalPrice = 0;
                updateCart();
            }
        }

        // 卡密验证
        document.getElementById('verifyForm').addEventListener('submit', function(event) {
            event.preventDefault();
            const cardCode = document.getElementById('cardCode').value;
            const verifyResult = document.getElementById('verifyResult');

            // 模拟卡密验证
            if (cardCode === "ABC123" || cardCode === "XYZ456") {
                verifyResult.textContent = "卡密有效！";
                verifyResult.style.color = "green";
            } else {
                verifyResult.textContent = "卡密无效，请检查后重试。";
                verifyResult.style.color = "red";
            }
        });

        // 初始化
        loadProducts();
    </script>
</body>
</html>
