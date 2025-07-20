<!DOCTYPE html>
<html lang="fr" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Benyahya AutoParts - Pièces détachées Opel</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #1e69b8; /* Opel blue */
            --secondary-color: #f7941d; /* Opel orange */
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .navbar {
            background-color: var(--primary-color);
        }
        .navbar-brand img {
            height: 40px;
        }
        .hero-section {
            background: linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)), url('https://images.unsplash.com/photo-1605559424843-9e4c228bf1c2?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1200&q=80');
            background-size: cover;
            background-position: center;
            color: white;
            padding: 100px 0;
            text-align: center;
        }
        .product-card {
            transition: transform 0.3s;
            margin-bottom: 20px;
            border: 1px solid #eee;
            border-radius: 8px;
            overflow: hidden;
        }
        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }
        .product-img {
            height: 200px;
            object-fit: contain;
            padding: 10px;
            background-color: #f9f9f9;
        }
        .price {
            color: var(--primary-color);
            font-weight: bold;
            font-size: 1.2rem;
        }
        .stock-badge {
            position: absolute;
            top: 10px;
            right: 10px;
        }
        .footer {
            background-color: #2c3e50;
            color: white;
            padding: 40px 0;
        }
        .language-switcher {
            cursor: pointer;
        }
        .hidden-stock {
            display: none;
        }
        .admin-view .hidden-stock {
            display: block;
            color: red;
            font-weight: bold;
        }
        .rtl-text {
            direction: rtl;
            text-align: right;
        }
        /* Admin specific styles */
        .admin-panel {
            display: none;
            background-color: #f8f9fa;
            padding: 20px;
            border-radius: 5px;
            margin-bottom: 30px;
        }
        .admin-login {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0,0,0,0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .login-box {
            background: white;
            padding: 30px;
            border-radius: 5px;
            width: 100%;
            max-width: 400px;
        }
        .admin-actions {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
        }
        .admin-btn {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
    </style>
</head>
<body>
    <!-- Admin Login Modal -->
    <div id="adminLogin" class="admin-login" style="display: none;">
        <div class="login-box">
            <h3 class="text-center mb-4" data-lang="fr">Connexion Admin</h3>
            <h3 class="text-center mb-4" data-lang="ar" style="display:none;">تسجيل الدخول للمشرف</h3>
            <div class="mb-3">
                <label class="form-label" data-lang="fr">Mot de passe</label>
                <label class="form-label" data-lang="ar" style="display:none;">كلمة المرور</label>
                <input type="password" id="adminPassword" class="form-control">
            </div>
            <button id="loginBtn" class="btn btn-primary w-100" data-lang="fr">Se connecter</button>
            <button id="loginBtn" class="btn btn-primary w-100" data-lang="ar" style="display:none;">تسجيل الدخول</button>
            <div id="loginError" class="text-danger mt-2" style="display: none;"></div>
        </div>
    </div>

    <!-- Admin Floating Button -->
    <div class="admin-actions">
        <button id="adminMenuBtn" class="btn btn-warning admin-btn">
            <i class="fas fa-cog"></i>
        </button>
    </div>

    <!-- Admin Panel -->
    <div id="adminPanel" class="admin-panel container mt-4">
        <h3 data-lang="fr">Panneau d'administration</h3>
        <h3 data-lang="ar" style="display:none;">لوحة التحكم</h3>

        <div class="row mt-4">
            <div class="col-md-6">
                <h4 data-lang="fr">Ajouter un nouveau produit</h4>
                <h4 data-lang="ar" style="display:none;">إضافة منتج جديد</h4>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Nom (Français)</label>
                    <label class="form-label" data-lang="ar" style="display:none;">الاسم (بالفرنسية)</label>
                    <input type="text" id="newProductName" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Nom (Arabe)</label>
                    <label class="form-label" data-lang="ar" style="display:none;">الاسم (بالعربية)</label>
                    <input type="text" id="newProductNameAr" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Modèle</label>
                    <label class="form-label" data-lang="ar" style="display:none;">الموديل</label>
                    <select id="newProductModel" class="form-select">
                        <option value="Corsa">Opel Corsa</option>
                        <option value="Astra">Opel Astra</option>
                        <option value="Insignia">Opel Insignia</option>
                        <option value="Vectra">Opel Vectra</option>
                        <option value="Zafira">Opel Zafira</option>
                        <option value="Meriva">Opel Meriva</option>
                        <option value="Mokka">Opel Mokka</option>
                        <option value="Crossland">Opel Crossland</option>
                        <option value="Grandland">Opel Grandland</option>
                        <option value="Combo">Opel Combo</option>
                        <option value="Movano">Opel Movano</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Catégorie</label>
                    <label class="form-label" data-lang="ar" style="display:none;">الفئة</label>
                    <select id="newProductCategory" class="form-select">
                        <option value="Moteur" data-lang="fr">Moteur</option>
                        <option value="Moteur" data-lang="ar" style="display:none;">المحرك</option>
                        <option value="Freinage" data-lang="fr">Freinage</option>
                        <option value="Freinage" data-lang="ar" style="display:none;">المكابح</option>
                        <option value="Suspension" data-lang="fr">Suspension</option>
                        <option value="Suspension" data-lang="ar" style="display:none;">التعليق</option>
                        <option value="Transmission" data-lang="fr">Transmission</option>
                        <option value="Transmission" data-lang="ar" style="display:none;">ناقل الحركة</option>
                        <option value="Électricité" data-lang="fr">Électricité</option>
                        <option value="Électricité" data-lang="ar" style="display:none;">الكهرباء</option>
                        <option value="Carrosserie" data-lang="fr">Carrosserie</option>
                        <option value="Carrosserie" data-lang="ar" style="display:none;">هيكل السيارة</option>
                        <option value="Filtration" data-lang="fr">Filtration</option>
                        <option value="Filtration" data-lang="ar" style="display:none;">الفلترة</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Prix (DZD)</label>
                    <label class="form-label" data-lang="ar" style="display:none;">السعر (دج)</label>
                    <input type="number" id="newProductPrice" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Image URL</label>
                    <label class="form-label" data-lang="ar" style="display:none;">رابط الصورة</label>
                    <input type="text" id="newProductImage" class="form-control" placeholder="https://example.com/image.jpg">
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Stock</label>
                    <label class="form-label" data-lang="ar" style="display:none;">المخزون</label>
                    <input type="number" id="newProductStock" class="form-control">
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Compatibilité (Français)</label>
                    <label class="form-label" data-lang="ar" style="display:none;">التوافق (بالفرنسية)</label>
                    <textarea id="newProductCompatible" class="form-control" rows="2"></textarea>
                </div>
                <div class="mb-3">
                    <label class="form-label" data-lang="fr">Compatibilité (Arabe)</label>
                    <label class="form-label" data-lang="ar" style="display:none;">التوافق (بالعربية)</label>
                    <textarea id="newProductCompatibleAr" class="form-control" rows="2"></textarea>
                </div>
                <button id="addProductBtn" class="btn btn-success" data-lang="fr">Ajouter Produit</button>
                <button id="addProductBtn" class="btn btn-success" data-lang="ar" style="display:none;">إضافة منتج</button>
            </div>
            <div class="col-md-6">
                <h4 data-lang="fr">Produits existants</h4>
                <h4 data-lang="ar" style="display:none;">المنتجات الحالية</h4>
                <div class="table-responsive">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th data-lang="fr">Nom</th>
                                <th data-lang="ar" style="display:none;">الاسم</th>
                                <th data-lang="fr">Modèle</th>
                                <th data-lang="ar" style="display:none;">الموديل</th>
                                <th data-lang="fr">Actions</th>
                                <th data-lang="ar" style="display:none;">الإجراءات</th>
                            </tr>
                        </thead>
                        <tbody id="productsTableBody">
                            <!-- Products will be inserted here -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark">
        <div class="container">
            <a class="navbar-brand" href="#">
                <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Opel_logo_2017.svg/1200px-Opel_logo_2017.svg.png" alt="Benyahya AutoParts">
                <span class="ms-2">Benyahya AutoParts</span>
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <a class="nav-link active" href="#" data-lang="fr">Accueil</a>
                        <a class="nav-link active" href="#" data-lang="ar" style="display:none;">الرئيسية</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#products" data-lang="fr">Produits</a>
                        <a class="nav-link" href="#products" data-lang="ar" style="display:none;">المنتجات</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#about" data-lang="fr">À propos</a>
                        <a class="nav-link" href="#about" data-lang="ar" style="display:none;">معلومات عنا</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#contact" data-lang="fr">Contact</a>
                        <a class="nav-link" href="#contact" data-lang="ar" style="display:none;">اتصل بنا</a>
                    </li>
                </ul>
                <div class="d-flex">
                    <div class="language-switcher me-3 text-white">
                        <span class="lang-fr">FR</span> / <span class="lang-ar">AR</span>
                    </div>
                    <form class="d-flex">
                        <input class="form-control me-2" type="search" placeholder="Rechercher..." data-lang="fr">
                        <input class="form-control me-2" type="search" placeholder="بحث..." data-lang="ar" style="display:none;">
                        <button class="btn btn-outline-light" type="submit">
                            <i class="fas fa-search"></i>
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section -->
    <section class="hero-section">
        <div class="container">
            <h1 data-lang="fr">Pièces détachées Opel en Algérie</h1>
            <h1 data-lang="ar" style="display:none;">قطع غيار أوبل في الجزائر</h1>
            <p class="lead" data-lang="fr">Toutes les pièces automobiles originales et compatibles pour votre véhicule Opel</p>
            <p class="lead" data-lang="ar" style="display:none;">جميع قطع غيار السيارات الأصلية والمتوافقة لسيارتك أوبل</p>
            <a href="#products" class="btn btn-primary btn-lg mt-3" data-lang="fr">Voir nos produits</a>
            <a href="#products" class="btn btn-primary btn-lg mt-3" data-lang="ar" style="display:none;">عرض منتجاتنا</a>
        </div>
    </section>

    <!-- Product Categories -->
    <section class="py-5 bg-light">
        <div class="container">
            <h2 class="text-center mb-5" data-lang="fr">Catégories de pièces</h2>
            <h2 class="text-center mb-5" data-lang="ar" style="display:none;">فئات القطع</h2>
            <div class="row">
                <div class="col-md-3 col-6 mb-4">
                    <div class="card product-card text-center">
                        <img src="https://images.unsplash.com/photo-1600861195091-690c92f1d2cc?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" class="card-img-top" alt="Moteur">
                        <div class="card-body">
                            <h5 class="card-title" data-lang="fr">Moteur</h5>
                            <h5 class="card-title" data-lang="ar" style="display:none;">المحرك</h5>
                        </div>
                    </div>
                </div>
                <div class="col-md-3 col-6 mb-4">
                    <div class="card product-card text-center">
                        <img src="https://images.unsplash.com/photo-1631729371254-42c2892f0e6e?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" class="card-img-top" alt="Freinage">
                        <div class="card-body">
                            <h5 class="card-title" data-lang="fr">Freinage</h5>
                            <h5 class="card-title" data-lang="ar" style="display:none;">المكابح</h5>
                        </div>
                    </div>
                </div>
                <div class="col-md-3 col-6 mb-4">
                    <div class="card product-card text-center">
                        <img src="https://images.unsplash.com/photo-1551632811-561732d1e306?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" class="card-img-top" alt="Suspension">
                        <div class="card-body">
                            <h5 class="card-title" data-lang="fr">Suspension</h5>
                            <h5 class="card-title" data-lang="ar" style="display:none;">التعليق</h5>
                        </div>
                    </div>
                </div>
                <div class="col-md-3 col-6 mb-4">
                    <div class="card product-card text-center">
                        <img src="https://images.unsplash.com/photo-1643015737353-50964a5b9754?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80" class="card-img-top" alt="Transmission">
                        <div class="card-body">
                            <h5 class="card-title" data-lang="fr">Transmission</h5>
                            <h5 class="card-title" data-lang="ar" style="display:none;">ناقل الحركة</h5>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="py-5">
        <div class="container">
            <h2 class="text-center mb-5" data-lang="fr">Nos pièces Opel</h2>
            <h2 class="text-center mb-5" data-lang="ar" style="display:none;">قطع أوبل لدينا</h2>

            <div class="row mb-4">
                <div class="col-md-6">
                    <select class="form-select" id="modelFilter">
                        <option value="" data-lang="fr">Tous les modèles</option>
                        <option value="" data-lang="ar" style="display:none;">جميع الموديلات</option>
                        <option value="Corsa">Opel Corsa</option>
                        <option value="Astra">Opel Astra</option>
                        <option value="Insignia">Opel Insignia</option>
                        <option value="Vectra">Opel Vectra</option>
                        <option value="Zafira">Opel Zafira</option>
                        <option value="Meriva">Opel Meriva</option>
                        <option value="Mokka">Opel Mokka</option>
                        <option value="Crossland">Opel Crossland</option>
                        <option value="Grandland">Opel Grandland</option>
                        <option value="Combo">Opel Combo</option>
                        <option value="Movano">Opel Movano</option>
                    </select>
                </div>
                <div class="col-md-6">
                    <select class="form-select" id="categoryFilter">
                        <option value="" data-lang="fr">Toutes les catégories</option>
                        <option value="" data-lang="ar" style="display:none;">جميع الفئات</option>
                        <option value="Moteur" data-lang="fr">Moteur</option>
                        <option value="Moteur" data-lang="ar" style="display:none;">المحرك</option>
                        <option value="Freinage" data-lang="fr">Freinage</option>
                        <option value="Freinage" data-lang="ar" style="display:none;">المكابح</option>
                        <option value="Suspension" data-lang="fr">Suspension</option>
                        <option value="Suspension" data-lang="ar" style="display:none;">التعليق</option>
                        <option value="Transmission" data-lang="fr">Transmission</option>
                        <option value="Transmission" data-lang="ar" style="display:none;">ناقل الحركة</option>
                        <option value="Électricité" data-lang="fr">Électricité</option>
                        <option value="Électricité" data-lang="ar" style="display:none;">الكهرباء</option>
                        <option value="Carrosserie" data-lang="fr">Carrosserie</option>
                        <option value="Carrosserie" data-lang="ar" style="display:none;">هيكل السيارة</option>
                        <option value="Filtration" data-lang="fr">Filtration</option>
                        <option value="Filtration" data-lang="ar" style="display:none;">الفلترة</option>
                    </select>
                </div>
            </div>

            <div class="row" id="productContainer">
                <!-- Product cards will be dynamically inserted here -->
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-5 bg-light">
        <div class="container">
            <div class="row">
                <div class="col-lg-6">
                    <h2 data-lang="fr">À propos de Benyahya AutoParts</h2>
                    <h2 data-lang="ar" style="display:none;">معلومات عن بن يحيى لقطع الغيار</h2>
                    <p data-lang="fr">
                        Benyahya AutoParts est le spécialiste des pièces détachées Opel en Algérie. Nous fournissons des pièces de qualité 
                        pour tous les modèles Opel. Avec des années d'expérience dans le domaine automobile, 
                        nous garantissons des produits originaux et compatibles à des prix compétitifs.
                    </p>
                    <p data-lang="ar" style="display:none;">
                        بن يحيى لقطع الغيار هو المتخصص في قطع غيار أوبل في الجزائر. نوفر قطع غيار عالية الجودة لجميع موديلات أوبل. 
                        مع سنوات من الخبرة في مجال السيارات، نضمن منتجات أصلية ومتوافقة بأسعار تنافسية.
                    </p>
                </div>
                <div class="col-lg-6">
                    <h2 data-lang="fr">Pourquoi choisir Benyahya AutoParts?</h2>
                    <h2 data-lang="ar" style="display:none;">لماذا تختار بن يحيى لقطع الغيار؟</h2>
                    <ul data-lang="fr">
                        <li>Pièces 100% compatibles avec votre véhicule Opel</li>
                        <li>Prix compétitifs en dinar algérien</li>
                        <li>Livraison rapide dans toute l'Algérie</li>
                        <li>Service client disponible 7j/7</li>
                        <li>Garantie sur toutes les pièces</li>
                        <li>Expertise technique spécialisée Opel</li>
                    </ul>
                    <ul data-lang="ar" style="display:none;">
                        <li>قطع غيار متوافقة 100% مع سيارتك أوبل</li>
                        <li>أسعار تنافسية بالدينار الجزائري</li>
                        <li>توصيل سريع في جميع أنحاء الجزائر</li>
                        <li>خدمة العملاء متاحة 7 أيام في الأسبوع</li>
                        <li>ضمان على جميع القطع</li>
                        <li>خبرة فنية متخصصة في أوبل</li>
                    </ul>
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-5">
        <div class="container">
            <h2 class="text-center mb-5" data-lang="fr">Contactez-nous</h2>
            <h2 class="text-center mb-5" data-lang="ar" style="display:none;">اتصل بنا</h2>
            <div class="row">
                <div class="col-lg-6 mb-4">
                    <div class="card">
                        <div class="card-body">
                            <h3 class="card-title" data-lang="fr">Nos coordonnées</h3>
                            <h3 class="card-title" data-lang="ar" style="display:none;">معلومات الاتصال</h3>
                            <p><i class="fas fa-map-marker-alt me-2"></i> Halaimi Mohamed, Khmis Meliana, Ain Defla, Algérie</p>
                            <p><i class="fas fa-phone me-2"></i> +213 782 065 342</p>
                            <p><i class="fas fa-envelope me-2"></i> ishak327k@gmail.com</p>
                            <div class="mt-4">
                                <h4 data-lang="fr">Heures d'ouverture</h4>
                                <h4 data-lang="ar" style="display:none;">ساعات العمل</h4>
                                <p data-lang="fr">Lundi - Samedi: 8h00 - 18h00</p>
                                <p data-lang="ar" style="display:none;">الإثنين - السبت: 8:00 - 18:00</p>
                                <p data-lang="fr">Dimanche: 9h00 - 13h00</p>
                                <p data-lang="ar" style="display:none;">الأحد: 9:00 - 13:00</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-6">
                    <form>
                        <div class="mb-3">
                            <input type="text" class="form-control" placeholder="Nom complet" data-lang="fr">
                            <input type="text" class="form-control" placeholder="الاسم الكامل" data-lang="ar" style="display:none;">
                        </div>
                        <div class="mb-3">
                            <input type="email" class="form-control" placeholder="Email" data-lang="fr">
                            <input type="email" class="form-control" placeholder="البريد الإلكتروني" data-lang="ar" style="display:none;">
                        </div>
                        <div class="mb-3">
                            <input type="tel" class="form-control" placeholder="Téléphone" data-lang="fr">
                            <input type="tel" class="form-control" placeholder="الهاتف" data-lang="ar" style="display:none;">
                        </div>
                        <div class="mb-3">
                            <textarea class="form-control" rows="4" placeholder="Votre message" data-lang="fr"></textarea>
                            <textarea class="form-control" rows="4" placeholder="رسالتك" data-lang="ar" style="display:none;"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary" data-lang="fr">Envoyer</button>
                        <button type="submit" class="btn btn-primary" data-lang="ar" style="display:none;">إرسال</button>
                    </form>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
        <div class="container">
            <div class="row">
                <div class="col-md-4 mb-4">
                    <h3 data-lang="fr">Benyahya AutoParts</h3>
                    <h3 data-lang="ar" style="display:none;">بنيحيى لقطع الغيار</h3>
                    <p data-lang="fr">Votre spécialiste en pièces détachées Opel en Algérie.</p>
                    <p data-lang="ar" style="display:none;">متخصصك في قطع غيار أوبل في الجزائر.</p>
                </div>
                <div class="col-md-4 mb-4">
                    <h3 data-lang="fr">Liens rapides</h3>
                    <h3 data-lang="ar" style="display:none;">روابط سريعة</h3>
                    <ul class="list-unstyled">
                        <li><a href="#" data-lang="fr">Accueil</a><a href="#" data-lang="ar" style="display:none;">الرئيسية</a></li>
                        <li><a href="#products" data-lang="fr">Produits</a><a href="#products" data-lang="ar" style="display:none;">المنتجات</a></li>
                        <li><a href="#about" data-lang="fr">À propos</a><a href="#about" data-lang="ar" style="display:none;">معلومات عنا</a></li>
                        <li><a href="#contact" data-lang="fr">Contact</a><a href="#contact" data-lang="ar" style="display:none;">اتصل بنا</a></li>
                    </ul>
                </div>
                <div class="col-md-4">
                    <h3 data-lang="fr">Suivez-nous</h3>
                    <h3 data-lang="ar" style="display:none;">تابعنا</h3>
                    <div class="social-links">
                        <a href="#" class="text-white me-3"><i class="fab fa-facebook-f"></i></a>
                        <a href="#" class="text-white me-3"><i class="fab fa-instagram"></i></a>
                        <a href="#" class="text-white me-3"><i class="fab fa-whatsapp"></i></a>
                    </div>
                </div>
            </div>
            <hr class="my-4 bg-light">
            <div class="row">
                <div class="col-md-6">
                    <p data-lang="fr">&copy; 2023 Benyahya AutoParts. Tous droits réservés.</p>
                    <p data-lang="ar" style="display:none;">&copy; 2023 بن يحيى لقطع الغيار. جميع الحقوق محفوظة.</p>
                </div>
                <div class="col-md-6 text-md-end">
                    <a href="#" class="text-white me-3" data-lang="fr">Conditions générales</a>
                    <a href="#" class="text-white me-3" data-lang="ar" style="display:none;">الشروط والأحكام</a>
                    <a href="#" class="text-white" data-lang="fr">Politique de confidentialité</a>
                    <a href="#" class="text-white" data-lang="ar" style="display:none;">سياسة الخصوصية</a>
                </div>
            </div>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        // Admin password - CHANGE THIS TO YOUR OWN SECURE PASSWORD
        const ADMIN_PASSWORD = "Benyahya123"; // You should change this!

        // Sample product data - will be loaded from localStorage
        let products = [];

        // Check if products exist in localStorage
        if (localStorage.getItem('benyahya-autoparts-products')) {
            products = JSON.parse(localStorage.getItem('benyahya-autoparts-products'));
        } else {
            // Initial products if none exist
            products = [
                {
                    id: 1,
                    name: "Filtre à huile",
                    name_ar: "فلتر الزيت",
                    model: "Corsa",
                    category: "Filtration",
                    category_ar: "الفلترة",
                    price: 2500,
                    image: "https://images.unsplash.com/photo-1600861195091-690c92f1d2cc?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80",
                    stock: 15,
                    compatible: "Opel Corsa D (2006-2014), Corsa E (2014-2019)",
                    compatible_ar: "أوبل كورسا D (2006-2014), كورسا E (2014-2019)"
                },
                {
                    id: 2,
                    name: "Plaquettes de frein avant",
                    name_ar: "وسادات الفرامل الأمامية",
                    model: "Astra",
                    category: "Freinage",
                    category_ar: "المكابح",
                    price: 8500,
                    image: "https://images.unsplash.com/photo-1631729371254-42c2892f0e6e?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80",
                    stock: 8,
                    compatible: "Opel Astra H (2004-2010), Astra J (2010-2015)",
                    compatible_ar: "أوبل أسترا H (2004-2010), أسترا J (2010-2015)"
                },
                {
                    id: 3,
                    name: "Amortisseur arrière",
                    name_ar: "ممتص الصدمات الخلفي",
                    model: "Vectra",
                    category: "Suspension",
                    category_ar: "التعليق",
                    price: 12000,
                    image: "https://images.unsplash.com/photo-1551632811-561732d1e306?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=500&q=80",
                    stock: 4,
                    compatible: "Opel Vectra C (2002-2008)",
                    compatible_ar: "أوبل فيكترا C (2002-2008)"
                }
            ];
            saveProducts();
        }

        // Admin state
        let isAdmin = false;

        // Language switching functionality
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize with French
            switchLanguage('fr');

            // Language switcher click event
            document.querySelector('.language-switcher').addEventListener('click', function() {
                const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';
                const newLang = currentLang === 'fr' ? 'ar' : 'fr';
                switchLanguage(newLang);
            });

            // Admin menu button
            document.getElementById('adminMenuBtn').addEventListener('click', function() {
                if (isAdmin) {
                    document.getElementById('adminPanel').style.display = 'block';
                } else {
                    document.getElementById('adminLogin').style.display = 'flex';
                }
            });

            // Login button
            document.getElementById('loginBtn').addEventListener('click', function() {
                const password = document.getElementById('adminPassword').value;
                if (password === ADMIN_PASSWORD) {
                    isAdmin = true;
                    document.getElementById('adminLogin').style.display = 'none';
                    document.getElementById('adminPanel').style.display = 'block';
                    document.body.classList.add('admin-view');
                    renderProductsTable();
                } else {
                    const errorEl = document.getElementById('loginError');
                    errorEl.textContent = document.querySelector('[data-lang="fr"]').style.display === 'none' 
                        ? 'كلمة المرور غير صحيحة' 
                        : 'Mot de passe incorrect';
                    errorEl.style.display = 'block';
                }
            });

            // Add product button
            document.getElementById('addProductBtn').addEventListener('click', function() {
                addNewProduct();
            });

            // Filter functionality
            document.getElementById('modelFilter').addEventListener('change', filterProducts);
            document.getElementById('categoryFilter').addEventListener('change', filterProducts);

            // Render products
            renderProducts(products);
        });

        function switchLanguage(lang) {
            // Hide all elements for both languages
            document.querySelectorAll('[data-lang="fr"]').forEach(el => {
                el.style.display = 'none';
            });
            document.querySelectorAll('[data-lang="ar"]').forEach(el => {
                el.style.display = 'none';
            });

            // Show elements for selected language
            document.querySelectorAll(`[data-lang="${lang}"]`).forEach(el => {
                el.style.display = '';
            });

            // Update dir attribute for Arabic
            if (lang === 'ar') {
                document.documentElement.dir = 'rtl';
                document.documentElement.lang = 'ar';
                document.body.classList.add('rtl-text');
            } else {
                document.documentElement.dir = 'ltr';
                document.documentElement.lang = 'fr';
                document.body.classList.remove('rtl-text');
            }

            // Re-render products with new language
            filterProducts();

            // Update admin panel if visible
            if (isAdmin) {
                renderProductsTable();
            }
        }

        function renderProducts(productsToRender) {
            const container = document.getElementById('productContainer');
            container.innerHTML = '';

            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            if (productsToRender.length === 0) {
                const noProducts = document.createElement('div');
                noProducts.className = 'col-12 text-center';
                noProducts.textContent = currentLang === 'fr' 
                    ? 'Aucun produit trouvé' 
                    : 'لم يتم العثور على منتجات';
                container.appendChild(noProducts);
                return;
            }

            productsToRender.forEach(product => {
                const stockStatus = product.stock > 0 
                    ? `<span class="badge bg-success">${currentLang === 'fr' ? 'En stock' : 'متوفر'}</span>` 
                    : `<span class="badge bg-danger">${currentLang === 'fr' ? 'Rupture' : 'نفذ'}</span>`;

                const stockInfo = `<div class="hidden-stock">Stock: ${product.stock}</div>`;

                const card = `
                    <div class="col-md-4 col-6 mb-4">
                        <div class="card product-card h-100">
                            ${stockStatus}
                            <img src="${product.image}" class="card-img-top product-img" alt="${currentLang === 'fr' ? product.name : product.name_ar}">
                            <div class="card-body">
                                <h5 class="card-title">${currentLang === 'fr' ? product.name : product.name_ar}</h5>
                                <p class="card-text"><strong>${currentLang === 'fr' ? 'Modèle:' : 'الموديل:'}</strong> Opel ${product.model}</p>
                                <p class="card-text"><strong>${currentLang === 'fr' ? 'Compatibilité:' : 'التوافق:'}</strong> ${currentLang === 'fr' ? product.compatible : product.compatible_ar}</p>
                                <p class="card-text"><strong>${currentLang === 'fr' ? 'Catégorie:' : 'الفئة:'}</strong> ${currentLang === 'fr' ? product.category : product.category_ar}</p>
                                <p class="price">${product.price.toLocaleString()} DZD</p>
                                ${stockInfo}
                                <button class="btn btn-primary w-100">${currentLang === 'fr' ? 'Commander' : 'اطلب الآن'}</button>
                            </div>
                        </div>
                    </div>
                `;

                container.innerHTML += card;
            });
        }

        function filterProducts() {
            const modelFilter = document.getElementById('modelFilter').value;
            const categoryFilter = document.getElementById('categoryFilter').value;
            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            let filteredProducts = products;

            if (modelFilter) {
                filteredProducts = filteredProducts.filter(product => product.model === modelFilter);
            }

            if (categoryFilter) {
                const categoryField = currentLang === 'fr' ? 'category' : 'category_ar';
                filteredProducts = filteredProducts.filter(product => product[categoryField] === categoryFilter);
            }

            renderProducts(filteredProducts);
        }

        function saveProducts() {
            localStorage.setItem('benyahya-autoparts-products', JSON.stringify(products));
        }

        function renderProductsTable() {
            const tbody = document.getElementById('productsTableBody');
            tbody.innerHTML = '';

            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            products.forEach(product => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${currentLang === 'fr' ? product.name : product.name_ar}</td>
                    <td>Opel ${product.model}</td>
                    <td>
                        <button class="btn btn-sm btn-primary edit-product" data-id="${product.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-product" data-id="${product.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            // Add event listeners to edit and delete buttons
            document.querySelectorAll('.edit-product').forEach(btn => {
                btn.addEventListener('click', function() {
                    const productId = parseInt(this.getAttribute('data-id'));
                    editProduct(productId);
                });
            });

            document.querySelectorAll('.delete-product').forEach(btn => {
                btn.addEventListener('click', function() {
                    const productId = parseInt(this.getAttribute('data-id'));
                    deleteProduct(productId);
                });
            });
        }

        function addNewProduct() {
            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            const newProduct = {
                id: products.length > 0 ? Math.max(...products.map(p => p.id)) + 1 : 1,
                name: document.getElementById('newProductName').value,
                name_ar: document.getElementById('newProductNameAr').value,
                model: document.getElementById('newProductModel').value,
                category: document.getElementById('newProductCategory').value,
                category_ar: document.getElementById('newProductCategory').options[document.getElementById('newProductCategory').selectedIndex].getAttribute('data-lang') === 'ar' 
                    ? document.getElementById('newProductCategory').value 
                    : '',
                price: parseInt(document.getElementById('newProductPrice').value),
                image: document.getElementById('newProductImage').value,
                stock: parseInt(document.getElementById('newProductStock').value),
                compatible: document.getElementById('newProductCompatible').value,
                compatible_ar: document.getElementById('newProductCompatibleAr').value
            };

            // Set Arabic category if not set
            if (!newProduct.category_ar) {
                const categorySelect = document.getElementById('newProductCategory');
                for (let i = 0; i < categorySelect.options.length; i++) {
                    if (categorySelect.options[i].value === newProduct.category && 
                        categorySelect.options[i].getAttribute('data-lang') === 'ar') {
                        newProduct.category_ar = categorySelect.options[i].text;
                        break;
                    }
                }
            }

            products.push(newProduct);
            saveProducts();
            renderProducts(products);
            renderProductsTable();

            // Clear form
            document.getElementById('newProductName').value = '';
            document.getElementById('newProductNameAr').value = '';
            document.getElementById('newProductPrice').value = '';
            document.getElementById('newProductImage').value = '';
            document.getElementById('newProductStock').value = '';
            document.getElementById('newProductCompatible').value = '';
            document.getElementById('newProductCompatibleAr').value = '';

            // Show success message
            alert(currentLang === 'fr' ? 'Produit ajouté avec succès!' : 'تمت إضافة المنتج بنجاح!');
        }

        function editProduct(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            const newName = prompt(currentLang === 'fr' ? 'Nouveau nom:' : 'الاسم الجديد:', product.name);
            if (newName !== null) {
                product.name = newName;
            }

            const newNameAr = prompt(currentLang === 'fr' ? 'Nouveau nom (Arabe):' : 'الاسم الجديد (عربي):', product.name_ar);
            if (newNameAr !== null) {
                product.name_ar = newNameAr;
            }

            const newPrice = prompt(currentLang === 'fr' ? 'Nouveau prix (DZD):' : 'السعر الجديد (دج):', product.price);
            if (newPrice !== null) {
                product.price = parseInt(newPrice);
            }

            const newStock = prompt(currentLang === 'fr' ? 'Nouveau stock:' : 'المخزون الجديد:', product.stock);
            if (newStock !== null) {
                product.stock = parseInt(newStock);
            }

            const newImage = prompt(currentLang === 'fr' ? 'Nouvelle image URL:' : 'رابط الصورة الجديدة:', product.image);
            if (newImage !== null) {
                product.image = newImage;
            }

            saveProducts();
            renderProducts(products);
            renderProductsTable();
        }

        function deleteProduct(productId) {
            const currentLang = document.querySelector('[data-lang="fr"]').style.display === 'none' ? 'ar' : 'fr';

            if (confirm(currentLang === 'fr' 
                ? 'Êtes-vous sûr de vouloir supprimer ce produit?' 
                : 'هل أنت متأكد أنك تريد حذف هذا المنتج؟')) {
                products = products.filter(p => p.id !== productId);
                saveProducts();
                renderProducts(products);
                renderProductsTable();
            }
        }
    </script>
</body>
</html>
