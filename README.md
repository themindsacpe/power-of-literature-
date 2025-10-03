<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>THE POWER OF WORDS</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-auth.js";
        import { getDatabase, ref, set, push, onValue, update, remove, get } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-database.js";
        import { getStorage, ref as storageRef, uploadBytesResumable, getDownloadURL, deleteObject } from "https://www.gstatic.com/firebasejs/9.23.0/firebase-storage.js";

        const firebaseConfig = {
            apiKey: "AIzaSyB3q47-D1b4xGkGtCAj9DOKeqX8hf2MtN8",
            authDomain: "literature-adc11.firebaseapp.com",
            databaseURL: "https://literature-adc11-default-rtdb.firebaseio.com",
            projectId: "literature-adc11",
            storageBucket: "literature-adc11.firebasestorage.app",
            messagingSenderId: "790500770691",
            appId: "1:790500770691:web:c86e27abfa782673fbacb6",
            measurementId: "G-MS8TFWD0WJ"
        };

        const app = initializeApp(firebaseConfig);
        
        const auth = getAuth(app);
        const db = getDatabase(app);
        const storage = getStorage(app);

        // Create default admin account on first load
        const createDefaultAdmin = async () => {
            const adminEmail = "admin@thoughtspower.com";
            const adminPassword = "admin123";
            
            try {
                // Try to create admin user
                const userCredential = await createUserWithEmailAndPassword(auth, adminEmail, adminPassword);
                const user = userCredential.user;
                
                // Add to admins collection
                const adminRef = ref(db, `admins/${user.uid}`);
                await set(adminRef, {
                    email: adminEmail,
                    name: "Website Administrator",
                    createdAt: new Date().toISOString(),
                    role: 'admin'
                });
                
                console.log("Default admin account created successfully");
            } catch (error) {
                if (error.code === 'auth/email-already-in-use') {
                    console.log("Admin account already exists");
                } else {
                    console.log("Admin setup completed");
                }
            }
        };

        // Initialize default admin
        createDefaultAdmin();

        window.firebaseAuth = auth;
        window.firebaseDB = db;
        window.firebaseStorage = storage;
        window.firebaseSignIn = signInWithEmailAndPassword;
        window.firebaseSignOut = signOut;
        window.firebaseOnAuthStateChanged = onAuthStateChanged;
        window.firebaseCreateUser = createUserWithEmailAndPassword;
        window.firebaseDBRef = ref;
        window.firebaseDBSet = set;
        window.firebaseDBPush = push;
        window.firebaseDBOnValue = onValue;
        window.firebaseDBUpdate = update;
        window.firebaseDBRemove = remove;
        window.firebaseDBGet = get;
        window.firebaseStorageRef = storageRef;
        window.firebaseUploadBytes = uploadBytesResumable;
        window.firebaseGetDownloadURL = getDownloadURL;
        window.firebaseDeleteObject = deleteObject;
        
        console.log("Firebase initialized successfully with admin setup");
    </script>
    
    <style>
        /* === CSS VARIABLES === */
        :root {
            --primary: #1a1a2e;
            --secondary: #16213e;
            --accent: #e94560;
            --accent-light: #ff6b9d;
            --light: #f8f9fa;
            --dark: #0f3460;
            --text: #2d3748;
            --text-light: #718096;
            --white: #ffffff;
            --success: #38b2ac;
            --warning: #ed8936;
            --danger: #e53e3e;
            --shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
            --shadow-lg: 0 20px 60px rgba(0, 0, 0, 0.15);
            --transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            --border-radius: 16px;
            --sidebar-width: 280px;
            --header-height: 80px;
            --gradient-primary: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            --gradient-accent: linear-gradient(135deg, var(--accent) 0%, var(--accent-light) 100%);
            --gradient-card: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
        }

        /* === BASE STYLES === */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Inter', sans-serif;
            line-height: 1.7;
            color: var(--text);
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }

        body.dark-mode {
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 100%);
            color: #e2e8f0;
        }

        h1, h2, h3, h4, h5 {
            font-family: 'Playfair Display', serif;
            margin-bottom: 1.5rem;
            color: var(--primary);
            line-height: 1.3;
            font-weight: 600;
        }

        body.dark-mode h1,
        body.dark-mode h2,
        body.dark-mode h3,
        body.dark-mode h4,
        body.dark-mode h5 {
            color: #f7fafc;
        }

        h1 {
            font-size: 3.5rem;
            background: var(--gradient-accent);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        h2 {
            font-size: 2.75rem;
        }

        h3 {
            font-size: 1.875rem;
        }

        p {
            margin-bottom: 1.5rem;
            color: var(--text);
            font-size: 1.1rem;
        }

        body.dark-mode p {
            color: #e2e8f0;
        }

        a {
            text-decoration: none;
            color: var(--accent);
            transition: var(--transition);
            font-weight: 500;
        }

        a:hover {
            color: var(--accent-light);
        }

        img {
            max-width: 100%;
            height: auto;
            border-radius: var(--border-radius);
            transition: var(--transition);
        }

        .container {
            width: 90%;
            max-width: 1200px;
            margin: 0 auto;
        }

        .btn {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            padding: 14px 28px;
            background: var(--gradient-accent);
            color: var(--white);
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            font-size: 1rem;
            transition: var(--transition);
            text-align: center;
            box-shadow: var(--shadow);
            position: relative;
            overflow: hidden;
        }

        .btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }

        .btn:hover::before {
            left: 100%;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: var(--shadow-lg);
        }

        .btn-accent {
            background: var(--gradient-accent);
        }

        .btn-success {
            background: linear-gradient(135deg, var(--success), #4fd1c7);
        }

        .btn-warning {
            background: linear-gradient(135deg, var(--warning), #f6ad55);
        }

        .btn-danger {
            background: linear-gradient(135deg, var(--danger), #fc8181);
        }

        .btn-outline {
            background: transparent;
            border: 2px solid var(--accent);
            color: var(--accent);
        }

        .btn-outline:hover {
            background: var(--accent);
            color: var(--white);
        }

        .btn-sm {
            padding: 10px 20px;
            font-size: 0.9rem;
        }

        .btn-lg {
            padding: 18px 36px;
            font-size: 1.1rem;
        }

        .btn-block {
            display: flex;
            width: 100%;
            justify-content: center;
        }

        .section {
            padding: 8rem 0;
            position: relative;
        }

        .section-title {
            text-align: center;
            margin-bottom: 5rem;
            position: relative;
        }

        .section-title::after {
            content: '';
            display: block;
            width: 120px;
            height: 4px;
            background: var(--gradient-accent);
            margin: 1.5rem auto;
            border-radius: 2px;
        }

        .card {
            background: var(--gradient-card);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            transition: var(--transition);
            border: 1px solid rgba(255, 255, 255, 0.3);
            backdrop-filter: blur(20px);
            position: relative;
        }

        body.dark-mode .card {
            background: linear-gradient(135deg, #2d3748 0%, #4a5568 100%);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .card:hover {
            transform: translateY(-10px);
            box-shadow: var(--shadow-lg);
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: var(--gradient-accent);
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }

        .card:hover::before {
            transform: scaleX(1);
        }

        .text-center { text-align: center; }
        .text-right { text-align: right; }
        .text-muted { color: var(--text-light); }

        body.dark-mode .text-muted {
            color: #a0aec0;
        }

        .mb-0 { margin-bottom: 0; }
        .mb-1 { margin-bottom: 0.75rem; }
        .mb-2 { margin-bottom: 1.5rem; }
        .mb-3 { margin-bottom: 2rem; }
        .mb-4 { margin-bottom: 3rem; }

        .mt-1 { margin-top: 0.75rem; }
        .mt-2 { margin-top: 1.5rem; }
        .mt-3 { margin-top: 2rem; }
        .mt-4 { margin-top: 3rem; }

        .d-flex { display: flex; }
        .justify-content-between { justify-content: space-between; }
        .align-items-center { align-items: center; }
        .w-100 { width: 100%; }

        .badge {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            padding: 8px 16px;
            background: var(--gradient-accent);
            color: var(--white);
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .badge-primary { background: var(--gradient-primary); }
        .badge-secondary { background: linear-gradient(135deg, #4a5568, #718096); }
        .badge-success { background: linear-gradient(135deg, var(--success), #4fd1c7); }

        .hidden { display: none !important; }

        /* === HEADER STYLES === */
        header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            box-shadow: var(--shadow);
            position: sticky;
            top: 0;
            z-index: 1000;
            border-bottom: 1px solid rgba(255, 255, 255, 0.3);
            transition: var(--transition);
        }

        body.dark-mode header {
            background: rgba(26, 32, 44, 0.95);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .header-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1.5rem 0;
        }

        .logo {
            font-family: 'Playfair Display', serif;
            font-size: 2.25rem;
            font-weight: 700;
            color: var(--primary);
            display: flex;
            align-items: center;
            gap: 12px;
        }

        body.dark-mode .logo {
            color: #f7fafc;
        }

        .logo i {
            color: var(--accent);
            font-size: 2.5rem;
        }

        .nav-menu {
            display: flex;
            list-style: none;
            gap: 2.5rem;
            align-items: center;
        }

        .nav-menu a {
            color: var(--primary);
            font-weight: 500;
            padding: 10px 20px;
            border-radius: 25px;
            transition: var(--transition);
            position: relative;
        }

        body.dark-mode .nav-menu a {
            color: #f7fafc;
        }

        .nav-menu a:hover {
            background: var(--gradient-accent);
            color: var(--white);
            transform: translateY(-2px);
        }

        .mobile-toggle {
            display: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--primary);
            transition: var(--transition);
        }

        body.dark-mode .mobile-toggle {
            color: #f7fafc;
        }

        /* === HERO SECTION === */
        .hero {
            background: var(--gradient-primary);
            color: var(--white);
            text-align: center;
            padding: 12rem 0;
            position: relative;
            overflow: hidden;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 1000"><polygon fill="%23ffffff08" points="0,1000 1000,0 1000,1000"/></svg>');
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-20px); }
        }

        .hero h1 {
            font-size: 4.5rem;
            margin-bottom: 2rem;
            color: var(--white);
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            -webkit-text-fill-color: var(--white);
            background: none;
        }

        .hero p {
            font-size: 1.4rem;
            max-width: 700px;
            margin: 0 auto 4rem;
            opacity: 0.9;
            color: var(--white);
        }

        .hero-buttons {
            display: flex;
            gap: 1.5rem;
            justify-content: center;
            flex-wrap: wrap;
        }

        /* === FEATURED ARTICLES === */
        .featured-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
            gap: 3rem;
        }

        .featured-article {
            position: relative;
            height: 450px;
            border-radius: var(--border-radius);
            overflow: hidden;
            cursor: pointer;
            box-shadow: var(--shadow);
        }

        .featured-article img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: var(--transition);
        }

        .featured-article:hover img {
            transform: scale(1.1);
        }

        .featured-content {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(transparent, rgba(0, 0, 0, 0.9));
            color: var(--white);
            padding: 2.5rem;
            transform: translateY(10px);
            transition: var(--transition);
        }

        .featured-article:hover .featured-content {
            transform: translateY(0);
        }

        .featured-content h3 {
            color: var(--white);
            margin-bottom: 1rem;
            font-size: 1.5rem;
        }

        /* === ARTICLES GRID === */
        .articles-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(420px, 1fr));
            gap: 3rem;
        }

        .article-card {
            display: flex;
            flex-direction: column;
            height: 100%;
            cursor: pointer;
        }

        .article-img {
            height: 280px;
            overflow: hidden;
            border-radius: var(--border-radius) var(--border-radius) 0 0;
        }

        .article-img img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: var(--transition);
        }

        .article-card:hover .article-img img {
            transform: scale(1.1);
        }

        .article-content {
            padding: 2.5rem;
            flex-grow: 1;
            display: flex;
            flex-direction: column;
        }

        .article-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: auto;
            padding-top: 1.5rem;
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            font-size: 0.9rem;
            color: var(--text-light);
        }

        body.dark-mode .article-meta {
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* === ADMIN PANEL STYLES === */
        .admin-panel {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--light);
            z-index: 2000;
            overflow-y: auto;
        }

        body.dark-mode .admin-panel {
            background: #1a202c;
        }

        .admin-header {
            background: var(--gradient-primary);
            color: var(--white);
            padding: 1.5rem 0;
            box-shadow: var(--shadow);
        }

        .admin-header-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .admin-logo {
            font-family: 'Playfair Display', serif;
            font-size: 1.8rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .admin-main {
            display: flex;
            min-height: calc(100vh - 80px);
        }

        .admin-sidebar {
            width: var(--sidebar-width);
            background: var(--white);
            box-shadow: var(--shadow);
            padding: 2.5rem 0;
        }

        body.dark-mode .admin-sidebar {
            background: #2d3748;
        }

        .sidebar-menu {
            list-style: none;
        }

        .sidebar-menu a {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 1.25rem 2.5rem;
            color: var(--text);
            transition: var(--transition);
            border-left: 4px solid transparent;
            font-weight: 500;
        }

        body.dark-mode .sidebar-menu a {
            color: #e2e8f0;
        }

        .sidebar-menu a:hover,
        .sidebar-menu a.active {
            background: rgba(233, 69, 96, 0.1);
            color: var(--accent);
            border-left-color: var(--accent);
        }

        .admin-content {
            flex: 1;
            padding: 3rem;
            background: #f8fafc;
        }

        body.dark-mode .admin-content {
            background: #1a202c;
        }

        .admin-section {
            display: none;
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            padding: 3rem;
            margin-bottom: 2rem;
            animation: fadeInUp 0.6s ease;
        }

        body.dark-mode .admin-section {
            background: #2d3748;
        }

        .admin-section.active {
            display: block;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .form-group {
            margin-bottom: 2rem;
        }

        .form-label {
            display: block;
            margin-bottom: 0.75rem;
            font-weight: 600;
            color: var(--primary);
            font-size: 1.1rem;
        }

        body.dark-mode .form-label {
            color: #f7fafc;
        }

        .form-control {
            width: 100%;
            padding: 1.25rem 1.5rem;
            border: 2px solid #e2e8f0;
            border-radius: var(--border-radius);
            font-size: 1rem;
            transition: var(--transition);
            background: var(--white);
            font-family: 'Inter', sans-serif;
        }

        body.dark-mode .form-control {
            background: #4a5568;
            border-color: #718096;
            color: #f7fafc;
        }

        .form-control:focus {
            border-color: var(--accent);
            outline: none;
            box-shadow: 0 0 0 3px rgba(233, 69, 96, 0.1);
            transform: translateY(-2px);
        }

        textarea.form-control {
            min-height: 200px;
            resize: vertical;
            line-height: 1.6;
        }

        .file-upload {
            position: relative;
            border: 3px dashed #cbd5e0;
            border-radius: var(--border-radius);
            padding: 4rem 2rem;
            text-align: center;
            transition: var(--transition);
            cursor: pointer;
            background: rgba(233, 69, 96, 0.02);
        }

        body.dark-mode .file-upload {
            border-color: #718096;
            background: rgba(233, 69, 96, 0.05);
        }

        .file-upload:hover {
            border-color: var(--accent);
            background: rgba(233, 69, 96, 0.08);
            transform: translateY(-2px);
        }

        .file-upload-input {
            position: absolute;
            left: 0;
            top: 0;
            opacity: 0;
            width: 100%;
            height: 100%;
            cursor: pointer;
        }

        .preview-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 1.5rem;
            margin-top: 1.5rem;
        }

        .preview-item {
            position: relative;
            border-radius: var(--border-radius);
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: var(--transition);
            aspect-ratio: 1;
        }

        .preview-item:hover {
            transform: scale(1.05);
            box-shadow: var(--shadow-lg);
        }

        .preview-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .preview-remove {
            position: absolute;
            top: 10px;
            right: 10px;
            background: var(--danger);
            color: white;
            border: none;
            border-radius: 50%;
            width: 35px;
            height: 35px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
            font-size: 1rem;
        }

        .preview-remove:hover {
            transform: scale(1.1);
            background: #c53030;
        }

        .table-responsive {
            overflow-x: auto;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
        }

        .table {
            width: 100%;
            border-collapse: collapse;
            background: var(--white);
            border-radius: var(--border-radius);
            overflow: hidden;
        }

        body.dark-mode .table {
            background: #2d3748;
        }

        .table th,
        .table td {
            padding: 1.5rem;
            text-align: left;
            border-bottom: 1px solid #e2e8f0;
        }

        body.dark-mode .table th,
        body.dark-mode .table td {
            border-bottom: 1px solid #4a5568;
        }

        .table th {
            background: var(--light);
            font-weight: 600;
            color: var(--primary);
            font-size: 1rem;
        }

        body.dark-mode .table th {
            background: #4a5568;
            color: #f7fafc;
        }

        .table tbody tr {
            transition: var(--transition);
        }

        .table tbody tr:hover {
            background: rgba(233, 69, 96, 0.05);
            transform: translateX(5px);
        }

        body.dark-mode .table tbody tr:hover {
            background: rgba(233, 69, 96, 0.1);
        }

        .table-actions {
            display: flex;
            gap: 0.75rem;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
            gap: 2rem;
            margin-bottom: 3rem;
        }

        .stat-card {
            background: var(--gradient-card);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            padding: 2.5rem;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.3);
            transition: var(--transition);
        }

        body.dark-mode .stat-card {
            background: linear-gradient(135deg, #2d3748 0%, #4a5568 100%);
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-lg);
        }

        .stat-icon {
            font-size: 3rem;
            margin-bottom: 1.5rem;
            color: var(--accent);
        }

        .stat-number {
            font-size: 3rem;
            font-weight: 700;
            margin-bottom: 0.5rem;
            background: var(--gradient-accent);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .stat-title {
            color: var(--text-light);
            font-size: 1.1rem;
            font-weight: 500;
        }

        /* === RESPONSIVE STYLES === */
        @media (max-width: 1024px) {
            .hero h1 {
                font-size: 3.5rem;
            }
            
            .featured-grid {
                grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            }
            
            .articles-grid {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 768px) {
            .nav-menu {
                display: none;
                position: absolute;
                top: 100%;
                left: 0;
                right: 0;
                background: var(--white);
                flex-direction: column;
                padding: 2rem;
                box-shadow: var(--shadow-lg);
                border-radius: 0 0 var(--border-radius) var(--border-radius);
            }

            body.dark-mode .nav-menu {
                background: #2d3748;
            }
            
            .nav-menu.active {
                display: flex;
            }
            
            .mobile-toggle {
                display: block;
            }
            
            .hero h1 {
                font-size: 2.75rem;
            }
            
            .hero p {
                font-size: 1.2rem;
            }
            
            .admin-main {
                flex-direction: column;
            }
            
            .admin-sidebar {
                width: 100%;
            }
            
            .admin-content {
                padding: 2rem;
            }
            
            .admin-section {
                padding: 2rem;
            }
        }

        @media (max-width: 480px) {
            .hero h1 {
                font-size: 2.25rem;
            }
            
            .featured-grid {
                grid-template-columns: 1fr;
            }
            
            .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .table-actions {
                flex-direction: column;
            }
        }

        /* === DARK MODE TOGGLE === */
        .dark-mode-toggle {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 65px;
            height: 65px;
            background: var(--gradient-accent);
            color: var(--white);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: var(--shadow-lg);
            z-index: 100;
            transition: var(--transition);
            font-size: 1.5rem;
        }

        .dark-mode-toggle:hover {
            transform: scale(1.1) rotate(15deg);
        }

        /* === MODAL STYLES === */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            z-index: 3000;
            align-items: center;
            justify-content: center;
            padding: 2rem;
            animation: fadeIn 0.3s ease;
        }

        .modal.active {
            display: flex;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .modal-content {
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow-lg);
            max-width: 900px;
            max-height: 90vh;
            overflow-y: auto;
            position: relative;
            animation: slideInUp 0.4s ease;
        }

        body.dark-mode .modal-content {
            background: #2d3748;
        }

        @keyframes slideInUp {
            from {
                opacity: 0;
                transform: translateY(50px) scale(0.9);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        .modal-close {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            background: var(--danger);
            color: white;
            border: none;
            border-radius: 50%;
            width: 45px;
            height: 45px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.25rem;
            z-index: 10;
            transition: var(--transition);
        }

        .modal-close:hover {
            transform: scale(1.1);
            background: #c53030;
        }

        /* === LOADING ANIMATION === */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: var(--white);
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* === NOTIFICATION STYLES === */
        .notification {
            position: fixed;
            top: 30px;
            right: 30px;
            padding: 1.25rem 1.75rem;
            background: var(--success);
            color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow-lg);
            z-index: 4000;
            display: flex;
            align-items: center;
            gap: 12px;
            animation: slideInRight 0.3s ease;
            max-width: 400px;
        }

        .notification.error {
            background: var(--danger);
        }

        .notification.warning {
            background: var(--warning);
        }

        @keyframes slideInRight {
            from {
                opacity: 0;
                transform: translateX(100%);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        /* === SEARCH BAR === */
        .search-bar {
            position: relative;
            max-width: 500px;
            margin: 0 auto 3rem;
        }

        .search-bar input {
            width: 100%;
            padding: 1.25rem 1.5rem 1.25rem 3.5rem;
            border: 2px solid #e2e8f0;
            border-radius: 50px;
            font-size: 1.1rem;
            transition: var(--transition);
            background: var(--white);
        }

        body.dark-mode .search-bar input {
            background: #4a5568;
            border-color: #718096;
            color: #f7fafc;
        }

        .search-bar input:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(233, 69, 96, 0.1);
        }

        .search-bar i {
            position: absolute;
            left: 1.5rem;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-light);
        }

        /* === EMPTY STATE STYLES === */
        .empty-state {
            text-align: center;
            padding: 4rem 2rem;
            color: var(--text-light);
        }

        .empty-state i {
            font-size: 4rem;
            margin-bottom: 1.5rem;
            opacity: 0.5;
        }

        .empty-state h3 {
            color: var(--text-light);
            margin-bottom: 1rem;
        }

        /* === PROGRESS BAR === */
        .progress-bar {
            width: 100%;
            height: 8px;
            background: #e2e8f0;
            border-radius: 4px;
            overflow: hidden;
            margin-top: 1rem;
        }

        .progress-fill {
            height: 100%;
            background: var(--gradient-accent);
            transition: width 0.3s ease;
        }

        /* === SHARE BUTTON === */
        .share-btn {
            background: var(--success);
            color: white;
            border: none;
            border-radius: 50px;
            padding: 10px 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 500;
            transition: var(--transition);
        }

        .share-btn:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow);
        }

        /* === BLOG STYLES === */
        .blog-section {
            background: var(--light);
            padding: 5rem 0;
        }

        body.dark-mode .blog-section {
            background: #1a202c;
        }

        .blog-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 2.5rem;
        }

        .blog-card {
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            transition: var(--transition);
        }

        body.dark-mode .blog-card {
            background: #2d3748;
        }

        .blog-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-lg);
        }

        .blog-content {
            padding: 2rem;
        }

        .blog-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 1.5rem;
            padding-top: 1.5rem;
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            font-size: 0.9rem;
            color: var(--text-light);
        }

        body.dark-mode .blog-meta {
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* === CONTENT WRITING SECTION === */
        .content-writing-section {
            background: var(--gradient-primary);
            color: var(--white);
            padding: 6rem 0;
            text-align: center;
        }

        .content-writing-section h2 {
            color: var(--white);
            margin-bottom: 2rem;
        }

        .writing-services {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2.5rem;
            margin-top: 4rem;
        }

        .writing-service {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: var(--border-radius);
            padding: 2.5rem;
            transition: var(--transition);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .writing-service:hover {
            transform: translateY(-10px);
            background: rgba(255, 255, 255, 0.15);
        }

        .writing-service i {
            font-size: 3rem;
            margin-bottom: 1.5rem;
            color: var(--accent-light);
        }

        /* === VIDEO SECTION === */
        .video-section {
            padding: 6rem 0;
        }

        .video-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 2.5rem;
        }

        .video-card {
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            transition: var(--transition);
        }

        body.dark-mode .video-card {
            background: #2d3748;
        }

        .video-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow-lg);
        }

        .video-placeholder {
            height: 200px;
            background: var(--gradient-primary);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--white);
            font-size: 4rem;
        }

        .video-content {
            padding: 1.5rem;
        }

        /* === REGISTER MODAL === */
        .auth-tabs {
            display: flex;
            margin-bottom: 2rem;
            border-bottom: 2px solid #e2e8f0;
        }

        .auth-tab {
            flex: 1;
            padding: 1rem;
            text-align: center;
            background: none;
            border: none;
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--text-light);
            cursor: pointer;
            transition: var(--transition);
        }

        .auth-tab.active {
            color: var(--accent);
            border-bottom: 3px solid var(--accent);
        }

        .auth-form {
            display: none;
        }

        .auth-form.active {
            display: block;
        }

        /* === CUSTOMER PANEL STYLES === */
        .customer-panel {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--light);
            z-index: 2000;
            overflow-y: auto;
        }

        body.dark-mode .customer-panel {
            background: #1a202c;
        }

        .customer-header {
            background: var(--gradient-primary);
            color: var(--white);
            padding: 1.5rem 0;
            box-shadow: var(--shadow);
        }

        .customer-header-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .customer-logo {
            font-family: 'Playfair Display', serif;
            font-size: 1.8rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .customer-main {
            display: flex;
            min-height: calc(100vh - 80px);
        }

        .customer-sidebar {
            width: var(--sidebar-width);
            background: var(--white);
            box-shadow: var(--shadow);
            padding: 2.5rem 0;
        }

        body.dark-mode .customer-sidebar {
            background: #2d3748;
        }

        .customer-section {
            display: none;
            background: var(--white);
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            padding: 3rem;
            margin-bottom: 2rem;
            animation: fadeInUp 0.6s ease;
        }

        body.dark-mode .customer-section {
            background: #2d3748;
        }

        .customer-section.active {
            display: block;
        }

        /* === COMMENT STYLES === */
        .comments-section {
            margin-top: 3rem;
            padding-top: 2rem;
            border-top: 1px solid rgba(0, 0, 0, 0.1);
        }

        body.dark-mode .comments-section {
            border-top: 1px solid rgba(255, 255, 255, 0.1);
        }

        .comment-form {
            margin-bottom: 2rem;
        }

        .comment-list {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .comment-item {
            background: rgba(233, 69, 96, 0.05);
            border-radius: var(--border-radius);
            padding: 1.5rem;
            border-left: 4px solid var(--accent);
        }

        body.dark-mode .comment-item {
            background: rgba(233, 69, 96, 0.1);
        }

        .comment-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 0.75rem;
        }

        .comment-author {
            font-weight: 600;
            color: var(--accent);
        }

        .comment-date {
            font-size: 0.85rem;
            color: var(--text-light);
        }

        .comment-content {
            line-height: 1.6;
        }

        /* === SETTINGS STYLES === */
        .settings-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
        }

        .contact-info {
            background: var(--gradient-card);
            border-radius: var(--border-radius);
            padding: 2rem;
            box-shadow: var(--shadow);
            margin-top: 3rem;
        }

        body.dark-mode .contact-info {
            background: linear-gradient(135deg, #2d3748 0%, #4a5568 100%);
        }

        .social-links {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        .social-link {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 45px;
            height: 45px;
            background: var(--accent);
            color: white;
            border-radius: 50%;
            transition: var(--transition);
        }

        .social-link:hover {
            transform: translateY(-3px);
            background: var(--accent-light);
        }

        /* === MESSAGING STYLES === */
        .messages-container {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            max-height: 600px;
            overflow-y: auto;
            padding: 1rem;
            border: 1px solid #e2e8f0;
            border-radius: var(--border-radius);
            background: var(--white);
        }

        body.dark-mode .messages-container {
            background: #2d3748;
            border-color: #4a5568;
        }

        .message {
            padding: 1rem 1.5rem;
            border-radius: var(--border-radius);
            max-width: 80%;
            position: relative;
            animation: fadeIn 0.3s ease;
        }

        .message.sent {
            align-self: flex-end;
            background: var(--gradient-accent);
            color: white;
        }

        .message.received {
            align-self: flex-start;
            background: #f7fafc;
            border: 1px solid #e2e8f0;
        }

        body.dark-mode .message.received {
            background: #4a5568;
            border-color: #718096;
        }

        .message-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 0.5rem;
            font-size: 0.85rem;
            opacity: 0.8;
        }

        .message-content {
            line-height: 1.5;
        }

        .message-form {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        .message-input {
            flex: 1;
            padding: 1rem 1.5rem;
            border: 2px solid #e2e8f0;
            border-radius: var(--border-radius);
            font-size: 1rem;
            transition: var(--transition);
            background: var(--white);
            font-family: 'Inter', sans-serif;
        }

        body.dark-mode .message-input {
            background: #4a5568;
            border-color: #718096;
            color: #f7fafc;
        }

        .message-input:focus {
            border-color: var(--accent);
            outline: none;
            box-shadow: 0 0 0 3px rgba(233, 69, 96, 0.1);
        }

        .conversation-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            max-height: 500px;
            overflow-y: auto;
        }

        .conversation-item {
            padding: 1.5rem;
            border-radius: var(--border-radius);
            background: var(--white);
            border: 1px solid #e2e8f0;
            cursor: pointer;
            transition: var(--transition);
        }

        body.dark-mode .conversation-item {
            background: #2d3748;
            border-color: #4a5568;
        }

        .conversation-item:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow);
            border-color: var(--accent);
        }

        .conversation-item.active {
            border-color: var(--accent);
            background: rgba(233, 69, 96, 0.05);
        }

        body.dark-mode .conversation-item.active {
            background: rgba(233, 69, 96, 0.1);
        }

        .conversation-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 0.5rem;
        }

        .conversation-name {
            font-weight: 600;
            color: var(--accent);
        }

        .conversation-date {
            font-size: 0.85rem;
            color: var(--text-light);
        }

        .conversation-preview {
            font-size: 0.9rem;
            color: var(--text-light);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .unread-badge {
            background: var(--accent);
            color: white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.75rem;
            font-weight: 600;
        }

        .messaging-layout {
            display: grid;
            grid-template-columns: 300px 1fr;
            gap: 2rem;
            height: 600px;
        }

        @media (max-width: 768px) {
            .messaging-layout {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <div class="container header-container">
            <a href="#" class="logo">
                <i class="fas fa-brain"></i>
                The Thoughts And Imagination Power 
            </a>
            <div class="mobile-toggle">
                <i class="fas fa-bars"></i>
            </div>
            <ul class="nav-menu">
                <li><a href="#home">Home</a></li>
                <li><a href="#articles">Daily Articles</a></li>
                <li><a href="#blog">Daily Blog</a></li>
                <li><a href="#literature">Literature</a></li>
                <li><a href="#content-writing">Content Writing</a></li>
                <li><a href="#media">Media</a></li>
                <li><a href="#" class="btn btn-accent" id="admin-login-btn">
                    <i class="fas fa-lock"></i>Admin Login
                </a></li>
                <li><a href="#" class="btn btn-outline" id="customer-login-btn">
                    <i class="fas fa-user"></i>Customer Login
                </a></li>
            </ul>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="hero" id="home">
        <div class="container">
            <h1>THE PEN IS MIGHTER THAN THE SWORD</h1>
            <p>CONTACT WITH WHATSAPP +92 324 7736384 .</p>
            <div class="hero-buttons">
                <a href="#articles" class="btn btn-lg">
                    <i class="fas fa-book-reader"></i>Explore Articles
                </a>
                <a href="#subscribe" class="btn btn-outline btn-lg">
                    <i class="fas fa-paper-plane"></i>Join Our Community
                </a>
            </div>
        </div>
    </section>

    <!-- Featured Articles -->
    <section class="section" id="articles">
        <div class="container">
            <h2 class="section-title">Daily Articles</h2>
            
            <!-- Search Bar -->
            <div class="search-bar">
                <i class="fas fa-search"></i>
                <input type="text" id="article-search" placeholder="Search for articles, topics, or authors...">
            </div>
            
            <div class="featured-grid" id="featured-articles-container">
                <div class="empty-state">
                    <i class="fas fa-newspaper"></i>
                    <h3>No Articles Yet</h3>
                    <p>Be the first to publish and share your thoughts with the world.</p>
                    <button class="btn btn-accent mt-3" onclick="document.getElementById('admin-login-btn').click()">
                        <i class="fas fa-pen"></i>Start Writing
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Daily Blog Section -->
    <section class="blog-section" id="blog">
        <div class="container">
            <h2 class="section-title">Daily Blog</h2>
            <div class="blog-grid" id="blog-container">
                <div class="empty-state">
                    <i class="fas fa-blog"></i>
                    <h3>No Blog Posts Yet</h3>
                    <p>Share your daily thoughts and experiences with our community.</p>
                    <button class="btn btn-accent mt-3" onclick="document.getElementById('admin-login-btn').click()">
                        <i class="fas fa-pen"></i>Write First Blog
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Literature Section -->
    <section class="section" id="literature">
        <div class="container">
            <h2 class="section-title">Literature Collection</h2>
            <div class="articles-grid" id="literature-container">
                <div class="empty-state">
                    <i class="fas fa-book"></i>
                    <h3>No Literature Pieces Yet</h3>
                    <p>Contribute to our growing collection of literary works.</p>
                    <button class="btn btn-accent mt-3" onclick="document.getElementById('admin-login-btn').click()">
                        <i class="fas fa-pen"></i>Contribute Literature
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Content Writing Section -->
    <section class="content-writing-section" id="content-writing">
        <div class="container">
            <h2>Content Writing Services</h2>
            <p>Professional content creation for your business, blog, or personal projects</p>
            
            <div class="writing-services">
                <div class="writing-service">
                    <i class="fas fa-file-alt"></i>
                    <h3>Article Writing</h3>
                    <p>Well-researched, engaging articles on any topic</p>
                </div>
                <div class="writing-service">
                    <i class="fas fa-blog"></i>
                    <h3>Blog Posts</h3>
                    <p>Regular blog content to keep your audience engaged</p>
                </div>
                <div class="writing-service">
                    <i class="fas fa-book"></i>
                    <h3>Creative Writing</h3>
                    <p>Original stories, poems, and creative pieces</p>
                </div>
                <div class="writing-service">
                    <i class="fas fa-briefcase"></i>
                    <h3>Business Content</h3>
                    <p>Professional content for websites, brochures, and marketing</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Media Section -->
    <section class="section" id="media">
        <div class="container">
            <h2 class="section-title">Media Gallery</h2>
            
            <!-- Images -->
            <div style="margin-bottom: 5rem;">
                <h3 style="text-align: center; margin-bottom: 3rem; color: var(--accent);">Images</h3>
                <div class="preview-container" id="public-media-gallery">
                    <div class="empty-state">
                        <i class="fas fa-images"></i>
                        <h3>No Images Yet</h3>
                        <p>Upload images to showcase in your articles and blog posts.</p>
                    </div>
                </div>
            </div>
            
            <!-- Videos -->
            <div>
                <h3 style="text-align: center; margin-bottom: 3rem; color: var(--accent);">Videos</h3>
                <div class="video-grid" id="video-container">
                    <div class="empty-state">
                        <i class="fas fa-video"></i>
                        <h3>No Videos Yet</h3>
                        <p>Share video content to complement your written work.</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Contact Info Section -->
    <section class="section" id="contact-info" style="background: var(--light);">
        <div class="container">
            <div class="contact-info">
                <h2 class="text-center" style="margin-bottom: 2rem;">Contact Information</h2>
                <div class="settings-grid">
                    <div>
                        <h3 style="color: var(--accent); margin-bottom: 1rem;">Store Information</h3>
                        <p><strong>Title:</strong> <span id="contact-title">The Power of Thoughts and Imagination</span></p>
                        <p><strong>Address:</strong> <span id="contact-address">Not specified</span></p>
                        <p><strong>Email:</strong> <span id="contact-email">samaddaud009@gmail.com</span></p>
                        <p><strong>Phone:</strong> <span id="contact-phone">+92 324 7736384</span></p>
                    </div>
                    <div>
                        <h3 style="color: var(--accent); margin-bottom: 1rem;">Owner Information</h3>
                        <p><strong>Name:</strong> <span id="contact-owner">Samad Daud</span></p>
                        <p><strong>Description:</strong> <span id="contact-description">Writer and Content Creator</span></p>
                    </div>
                </div>
                <div class="social-links">
                    <a href="#" class="social-link" id="youtube-link" title="YouTube">
                        <i class="fab fa-youtube"></i>
                    </a>
                    <a href="#" class="social-link" id="whatsapp-link" title="WhatsApp">
                        <i class="fab fa-whatsapp"></i>
                    </a>
                    <a href="#" class="social-link" id="facebook-link" title="Facebook">
                        <i class="fab fa-facebook"></i>
                    </a>
                </div>
            </div>
        </div>
    </section>

    <!-- Admin Panel -->
    <div class="admin-panel" id="admin-panel">
        <div class="admin-header">
            <div class="container admin-header-container">
                <div class="admin-logo">
                    <i class="fas fa-cogs"></i>
                    Admin Dashboard
                </div>
                <div class="admin-nav">
                    <span style="margin-right: 1rem; opacity: 0.9;" id="admin-user-email">Welcome, Admin</span>
                    <button class="btn btn-outline btn-sm" id="admin-logout">
                        <i class="fas fa-sign-out-alt"></i>Logout
                    </button>
                </div>
            </div>
        </div>

        <div class="admin-main">
            <div class="admin-sidebar">
                <ul class="sidebar-menu">
                    <li><a href="#" class="active" data-section="dashboard">
                        <i class="fas fa-tachometer-alt"></i>Dashboard
                    </a></li>
                    <li><a href="#" data-section="articles">
                        <i class="fas fa-newspaper"></i>Article Management
                    </a></li>
                    <li><a href="#" data-section="blog">
                        <i class="fas fa-blog"></i>Blog Management
                    </a></li>
                    <li><a href="#" data-section="literature">
                        <i class="fas fa-book"></i>Literature Management
                    </a></li>
                    <li><a href="#" data-section="content-writing">
                        <i class="fas fa-pen-fancy"></i>Content Writing
                    </a></li>
                    <li><a href="#" data-section="customers">
                        <i class="fas fa-users"></i>Customer Management
                    </a></li>
                    <li><a href="#" data-section="messaging">
                        <i class="fas fa-comments"></i>Messaging
                    </a></li>
                    <li><a href="#" data-section="media">
                        <i class="fas fa-images"></i>Media Library
                    </a></li>
                    <li><a href="#" data-section="settings">
                        <i class="fas fa-cog"></i>Settings
                    </a></li>
                    <li><a href="#" data-section="analytics">
                        <i class="fas fa-chart-bar"></i>Analytics
                    </a></li>
                </ul>
            </div>

            <div class="admin-content">
                <!-- Dashboard Section -->
                <div class="admin-section active" id="dashboard-section">
                    <h2 style="margin-bottom: 3rem;">Dashboard Overview</h2>
                    <div class="stats-grid">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-newspaper"></i>
                            </div>
                            <div class="stat-number" id="total-articles">0</div>
                            <div class="stat-title">Published Articles</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-blog"></i>
                            </div>
                            <div class="stat-number" id="total-blogs">0</div>
                            <div class="stat-title">Blog Posts</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-users"></i>
                            </div>
                            <div class="stat-number" id="total-customers">0</div>
                            <div class="stat-title">Registered Customers</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-images"></i>
                            </div>
                            <div class="stat-number" id="total-media">0</div>
                            <div class="stat-title">Media Files</div>
                        </div>
                    </div>
                    
                    <div class="card" style="padding: 2.5rem; margin-top: 3rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Quick Actions</h3>
                        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1.5rem;">
                            <button class="btn btn-success" onclick="showAddArticleForm()">
                                <i class="fas fa-plus"></i>Create Article
                            </button>
                            <button class="btn btn-success" onclick="showAddBlogForm()">
                                <i class="fas fa-plus"></i>Create Blog Post
                            </button>
                            <button class="btn btn-success" onclick="showAddLiteratureForm()">
                                <i class="fas fa-plus"></i>Add Literature
                            </button>
                            <button class="btn btn-success" onclick="showMediaSection()">
                                <i class="fas fa-plus"></i>Upload Media
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Articles Management Section -->
                <div class="admin-section" id="articles-section">
                    <div class="d-flex justify-content-between align-items-center mb-4">
                        <h2 style="margin: 0;">Article Management</h2>
                        <button class="btn btn-success" id="add-article-btn">
                            <i class="fas fa-plus"></i>Create New Article
                        </button>
                    </div>
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Title</th>
                                    <th>Category</th>
                                    <th>Date</th>
                                    <th>Views</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="articles-table-body">
                                <tr>
                                    <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                                        <i class="fas fa-newspaper" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                                        <h4>No Articles Found</h4>
                                        <p>Create your first article to get started!</p>
                                        <button class="btn btn-success mt-2" id="empty-state-create-btn">
                                            <i class="fas fa-plus"></i>Create First Article
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Blog Management Section -->
                <div class="admin-section" id="blog-section">
                    <div class="d-flex justify-content-between align-items-center mb-4">
                        <h2 style="margin: 0;">Blog Management</h2>
                        <button class="btn btn-success" id="add-blog-btn">
                            <i class="fas fa-plus"></i>Create New Blog Post
                        </button>
                    </div>
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Title</th>
                                    <th>Category</th>
                                    <th>Date</th>
                                    <th>Views</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="blog-table-body">
                                <tr>
                                    <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                                        <i class="fas fa-blog" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                                        <h4>No Blog Posts Found</h4>
                                        <p>Create your first blog post to get started!</p>
                                        <button class="btn btn-success mt-2" id="empty-state-create-blog-btn">
                                            <i class="fas fa-plus"></i>Create First Blog
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Literature Management Section -->
                <div class="admin-section" id="literature-section">
                    <div class="d-flex justify-content-between align-items-center mb-4">
                        <h2 style="margin: 0;">Literature Management</h2>
                        <button class="btn btn-success" id="add-literature-btn">
                            <i class="fas fa-plus"></i>Add New Literature
                        </button>
                    </div>
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Title</th>
                                    <th>Type</th>
                                    <th>Author</th>
                                    <th>Date</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="literature-table-body">
                                <tr>
                                    <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                                        <i class="fas fa-book" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                                        <h4>No Literature Found</h4>
                                        <p>Add your first literature piece to get started!</p>
                                        <button class="btn btn-success mt-2" id="empty-state-create-literature-btn">
                                            <i class="fas fa-plus"></i>Add First Literature
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Content Writing Section -->
                <div class="admin-section" id="content-writing-section">
                    <h2 style="margin-bottom: 3rem;">Content Writing Services</h2>
                    
                    <div class="card" style="padding: 2.5rem; margin-bottom: 3rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Content Writing Projects</h3>
                        <div class="table-responsive">
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th>Project Name</th>
                                        <th>Client</th>
                                        <th>Type</th>
                                        <th>Deadline</th>
                                        <th>Status</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="content-writing-table-body">
                                    <tr>
                                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                                            <i class="fas fa-pen-fancy" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                                            <h4>No Content Writing Projects</h4>
                                            <p>Start your first content writing project!</p>
                                            <button class="btn btn-success mt-2" id="empty-state-create-content-btn">
                                                <i class="fas fa-plus"></i>Start New Project
                                            </button>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    <div class="card" style="padding: 2.5rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Start New Content Writing Project</h3>
                        <form id="content-writing-form">
                            <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                                <div class="form-group">
                                    <label class="form-label">Project Name</label>
                                    <input type="text" class="form-control" id="project-name" placeholder="Enter project name" required>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Client Name</label>
                                    <input type="text" class="form-control" id="client-name" placeholder="Enter client name" required>
                                </div>
                            </div>
                            
                            <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                                <div class="form-group">
                                    <label class="form-label">Content Type</label>
                                    <select class="form-control" id="content-type" required>
                                        <option value="">Choose content type</option>
                                        <option value="article">Article</option>
                                        <option value="blog">Blog Post</option>
                                        <option value="website">Website Content</option>
                                        <option value="creative">Creative Writing</option>
                                        <option value="business">Business Content</option>
                                    </select>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Deadline</label>
                                    <input type="date" class="form-control" id="project-deadline" required>
                                </div>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label">Project Description</label>
                                <textarea class="form-control" id="project-description" placeholder="Describe the project requirements..." rows="5" required></textarea>
                            </div>
                            
                            <div class="d-flex justify-content-end">
                                <button type="submit" class="btn btn-success">
                                    <i class="fas fa-paper-plane"></i>Create Project
                                </button>
                            </div>
                        </form>
                    </div>
                </div>

                <!-- Customer Management Section -->
                <div class="admin-section" id="customers-section">
                    <div class="d-flex justify-content-between align-items-center mb-4">
                        <h2 style="margin: 0;">Customer Management</h2>
                        <button class="btn btn-success" id="add-customer-btn">
                            <i class="fas fa-plus"></i>Add New Customer
                        </button>
                    </div>
                    <div class="table-responsive">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Full Name</th>
                                    <th>Father Name</th>
                                    <th>Phone</th>
                                    <th>Email</th>
                                    <th>Address</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="customers-table-body">
                                <tr>
                                    <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                                        <i class="fas fa-users" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                                        <h4>No Customers Found</h4>
                                        <p>No customers have registered yet.</p>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Messaging Section -->
                <div class="admin-section" id="messaging-section">
                    <h2 style="margin-bottom: 3rem;">Messaging System</h2>
                    
                    <div class="messaging-layout">
                        <!-- Conversations List -->
                        <div class="card" style="padding: 1.5rem;">
                            <h3 style="margin-bottom: 1.5rem; color: var(--accent);">Conversations</h3>
                            <div class="conversation-list" id="admin-conversations-list">
                                <div class="empty-state">
                                    <i class="fas fa-comments"></i>
                                    <h3>No Conversations</h3>
                                    <p>Start a conversation with a customer.</p>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Messages Area -->
                        <div class="card" style="padding: 1.5rem; display: flex; flex-direction: column;">
                            <div id="admin-messages-header" style="margin-bottom: 1.5rem; padding-bottom: 1rem; border-bottom: 1px solid #e2e8f0;">
                                <h3 style="margin: 0; color: var(--accent);">Select a conversation</h3>
                            </div>
                            
                            <div class="messages-container" id="admin-messages-container">
                                <div class="empty-state">
                                    <i class="fas fa-comment"></i>
                                    <h3>No Messages</h3>
                                    <p>Select a conversation to view messages.</p>
                                </div>
                            </div>
                            
                            <form class="message-form" id="admin-message-form" style="display: none;">
                                <input type="hidden" id="admin-current-conversation">
                                <input type="text" class="message-input" id="admin-message-input" placeholder="Type your message..." required>
                                <button type="submit" class="btn btn-success">
                                    <i class="fas fa-paper-plane"></i>Send
                                </button>
                            </form>
                        </div>
                    </div>
                </div>

                <!-- Settings Section -->
                <div class="admin-section" id="settings-section">
                    <h2 style="margin-bottom: 3rem;">Store Settings</h2>
                    <form id="settings-form">
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Store Title</label>
                                <input type="text" class="form-control" id="store-title" placeholder="Enter store title" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Owner Name</label>
                                <input type="text" class="form-control" id="owner-name" placeholder="Enter owner name" required>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Store Address</label>
                            <textarea class="form-control" id="store-address" placeholder="Enter store address" rows="3" required></textarea>
                        </div>
                        
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Email Address</label>
                                <input type="email" class="form-control" id="store-email" placeholder="Enter email address" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Phone Number</label>
                                <input type="tel" class="form-control" id="store-phone" placeholder="Enter phone number" required>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Store Description</label>
                            <textarea class="form-control" id="store-description" placeholder="Enter store description" rows="4" required></textarea>
                        </div>
                        
                        <h3 style="margin: 3rem 0 2rem; color: var(--accent);">Social Media Links</h3>
                        
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">YouTube Channel</label>
                                <input type="url" class="form-control" id="youtube-channel" placeholder="Enter YouTube channel URL">
                            </div>
                            <div class="form-group">
                                <label class="form-label">WhatsApp Channel</label>
                                <input type="url" class="form-control" id="whatsapp-channel" placeholder="Enter WhatsApp channel URL">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Facebook Page</label>
                                <input type="url" class="form-control" id="facebook-page" placeholder="Enter Facebook page URL">
                            </div>
                        </div>
                        
                        <div class="d-flex justify-content-end">
                            <button type="submit" class="btn btn-success">
                                <i class="fas fa-save"></i>Save Settings
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Add/Edit Article Form -->
                <div class="admin-section" id="add-article-section">
                    <h2 style="margin-bottom: 3rem;" id="article-form-title">Create New Article</h2>
                    <form id="article-form">
                        <input type="hidden" id="article-id">
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Article Title</label>
                                <input type="text" class="form-control" id="article-title" placeholder="Enter captivating title" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Category</label>
                                <select class="form-control" id="article-category" required>
                                    <option value="">Choose category</option>
                                    <option value="philosophy">Philosophy</option>
                                    <option value="psychology">Psychology</option>
                                    <option value="creativity">Creativity</option>
                                    <option value="mindfulness">Mindfulness</option>
                                    <option value="literature">Literature</option>
                                    <option value="science">Science</option>
                                </select>
                            </div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Featured Image</label>
                            <div class="file-upload">
                                <input type="file" class="file-upload-input" id="featured-image" accept="image/*">
                                <div>
                                    <i class="fas fa-cloud-upload-alt" style="font-size: 3.5rem; color: var(--accent); margin-bottom: 1.5rem;"></i>
                                    <p style="margin-bottom: 0.75rem; font-weight: 600; font-size: 1.2rem;">Upload Featured Image</p>
                                    <p class="text-muted">Drag & drop or click to browse (PNG, JPG, GIF up to 10MB)</p>
                                </div>
                            </div>
                            <div class="preview-container" id="featured-image-preview"></div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Article Content</label>
                            <textarea class="form-control" id="article-content" placeholder="Write your inspiring content here... (Supports multiple paragraphs)" rows="15" required></textarea>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Tags (Optional)</label>
                            <input type="text" class="form-control" id="article-tags" placeholder="Add relevant tags separated by commas">
                        </div>

                        <div class="d-flex justify-content-between align-items-center mt-4">
                            <button type="button" class="btn btn-outline" id="cancel-article-btn">
                                <i class="fas fa-arrow-left"></i>Back to Articles
                            </button>
                            <button type="submit" class="btn btn-success" id="save-article-btn">
                                <i class="fas fa-paper-plane"></i>Publish Article
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Add/Edit Blog Form -->
                <div class="admin-section" id="add-blog-section">
                    <h2 style="margin-bottom: 3rem;" id="blog-form-title">Create New Blog Post</h2>
                    <form id="blog-form">
                        <input type="hidden" id="blog-id">
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Blog Title</label>
                                <input type="text" class="form-control" id="blog-title" placeholder="Enter blog title" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Category</label>
                                <select class="form-control" id="blog-category" required>
                                    <option value="">Choose category</option>
                                    <option value="personal">Personal</option>
                                    <option value="professional">Professional</option>
                                    <option value="creative">Creative</option>
                                    <option value="technical">Technical</option>
                                    <option value="lifestyle">Lifestyle</option>
                                </select>
                            </div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Featured Image</label>
                            <div class="file-upload">
                                <input type="file" class="file-upload-input" id="blog-featured-image" accept="image/*">
                                <div>
                                    <i class="fas fa-cloud-upload-alt" style="font-size: 3.5rem; color: var(--accent); margin-bottom: 1.5rem;"></i>
                                    <p style="margin-bottom: 0.75rem; font-weight: 600; font-size: 1.2rem;">Upload Featured Image</p>
                                    <p class="text-muted">Drag & drop or click to browse (PNG, JPG, GIF up to 10MB)</p>
                                </div>
                            </div>
                            <div class="preview-container" id="blog-featured-image-preview"></div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Blog Content</label>
                            <textarea class="form-control" id="blog-content" placeholder="Write your blog content here..." rows="15" required></textarea>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Tags (Optional)</label>
                            <input type="text" class="form-control" id="blog-tags" placeholder="Add relevant tags separated by commas">
                        </div>

                        <div class="d-flex justify-content-between align-items-center mt-4">
                            <button type="button" class="btn btn-outline" id="cancel-blog-btn">
                                <i class="fas fa-arrow-left"></i>Back to Blog
                            </button>
                            <button type="submit" class="btn btn-success" id="save-blog-btn">
                                <i class="fas fa-paper-plane"></i>Publish Blog
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Add/Edit Literature Form -->
                <div class="admin-section" id="add-literature-section">
                    <h2 style="margin-bottom: 3rem;" id="literature-form-title">Add New Literature</h2>
                    <form id="literature-form">
                        <input type="hidden" id="literature-id">
                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Title</label>
                                <input type="text" class="form-control" id="literature-title" placeholder="Enter literature title" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Type</label>
                                <select class="form-control" id="literature-type" required>
                                    <option value="">Choose type</option>
                                    <option value="poetry">Poetry</option>
                                    <option value="short-story">Short Story</option>
                                    <option value="essay">Essay</option>
                                    <option value="novel">Novel Excerpt</option>
                                    <option value="drama">Drama</option>
                                </select>
                            </div>
                        </div>

                        <div class="form-row" style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-bottom: 2rem;">
                            <div class="form-group">
                                <label class="form-label">Author</label>
                                <input type="text" class="form-control" id="literature-author" placeholder="Enter author name" required>
                            </div>
                            <div class="form-group">
                                <label class="form-label">Publication Year</label>
                                <input type="number" class="form-control" id="literature-year" placeholder="Enter publication year" min="1000" max="2030">
                            </div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Cover Image</label>
                            <div class="file-upload">
                                <input type="file" class="file-upload-input" id="literature-cover-image" accept="image/*">
                                <div>
                                    <i class="fas fa-cloud-upload-alt" style="font-size: 3.5rem; color: var(--accent); margin-bottom: 1.5rem;"></i>
                                    <p style="margin-bottom: 0.75rem; font-weight: 600; font-size: 1.2rem;">Upload Cover Image</p>
                                    <p class="text-muted">Drag & drop or click to browse (PNG, JPG, GIF up to 10MB)</p>
                                </div>
                            </div>
                            <div class="preview-container" id="literature-cover-image-preview"></div>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Content</label>
                            <textarea class="form-control" id="literature-content" placeholder="Enter the literature content..." rows="15" required></textarea>
                        </div>

                        <div class="form-group">
                            <label class="form-label">Summary (Optional)</label>
                            <textarea class="form-control" id="literature-summary" placeholder="Enter a brief summary..." rows="3"></textarea>
                        </div>

                        <div class="d-flex justify-content-between align-items-center mt-4">
                            <button type="button" class="btn btn-outline" id="cancel-literature-btn">
                                <i class="fas fa-arrow-left"></i>Back to Literature
                            </button>
                            <button type="submit" class="btn btn-success" id="save-literature-btn">
                                <i class="fas fa-paper-plane"></i>Save Literature
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Media Library Section -->
                <div class="admin-section" id="media-section">
                    <h2 style="margin-bottom: 3rem;">Media Library</h2>
                    
                    <div class="card" style="margin-bottom: 3rem; padding: 2.5rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Upload New Media</h3>
                        <div class="file-upload">
                            <input type="file" class="file-upload-input" id="media-files" accept="image/*,video/*" multiple>
                            <div>
                                <i class="fas fa-images" style="font-size: 3.5rem; color: var(--accent); margin-bottom: 1.5rem;"></i>
                                <p style="margin-bottom: 0.75rem; font-weight: 600; font-size: 1.2rem;">Upload Multiple Images & Videos</p>
                                <p class="text-muted">Drag & drop or click to browse multiple files (PNG, JPG, GIF, MP4 up to 100MB)</p>
                            </div>
                        </div>
                        <div id="upload-progress-container" style="display: none;">
                            <div class="progress-bar">
                                <div class="progress-fill" id="upload-progress" style="width: 0%"></div>
                            </div>
                            <p class="text-muted mt-1" id="upload-status">Uploading...</p>
                        </div>
                        <div class="preview-container" id="media-preview"></div>
                    </div>
                    
                    <div class="card" style="padding: 2.5rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Media Gallery</h3>
                        <div class="preview-container" id="media-gallery">
                            <div class="empty-state">
                                <i class="fas fa-images"></i>
                                <h3>No Media Files</h3>
                                <p>Upload images and videos to use in your articles.</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Analytics Section -->
                <div class="admin-section" id="analytics-section">
                    <h2 style="margin-bottom: 3rem;">Platform Analytics</h2>
                    <div class="stats-grid">
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-chart-line"></i>
                            </div>
                            <div class="stat-number" id="monthly-views">0</div>
                            <div class="stat-title">Monthly Views</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-user-clock"></i>
                            </div>
                            <div class="stat-number" id="avg-read-time">0m</div>
                            <div class="stat-title">Avg. Read Time</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-share-alt"></i>
                            </div>
                            <div class="stat-number" id="total-shares">0</div>
                            <div class="stat-title">Social Shares</div>
                        </div>
                        <div class="stat-card">
                            <div class="stat-icon">
                                <i class="fas fa-heart"></i>
                            </div>
                            <div class="stat-number" id="total-likes">0</div>
                            <div class="stat-title">Total Likes</div>
                        </div>
                    </div>
                    
                    <div class="card" style="padding: 2.5rem; margin-top: 3rem;">
                        <h3 style="margin-bottom: 2rem; color: var(--accent);">Content Performance</h3>
                        <div class="table-responsive">
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th>Content Title</th>
                                        <th>Type</th>
                                        <th>Views</th>
                                        <th>Likes</th>
                                        <th>Shares</th>
                                        <th>Performance</th>
                                    </tr>
                                </thead>
                                <tbody id="analytics-table-body">
                                    <tr>
                                        <td colspan="6" class="text-center text-muted" style="padding: 2rem;">
                                            <i class="fas fa-chart-bar" style="font-size: 2.5rem; margin-bottom: 1rem; display: block;"></i>
                                            <p>No content data available yet</p>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Customer Panel -->
    <div class="customer-panel" id="customer-panel">
        <div class="customer-header">
            <div class="container customer-header-container">
                <div class="customer-logo">
                    <i class="fas fa-user"></i>
                    Customer Dashboard
                </div>
                <div class="customer-nav">
                    <span style="margin-right: 1rem; opacity: 0.9;" id="customer-user-name">Welcome, Customer</span>
                    <button class="btn btn-outline btn-sm" id="customer-logout">
                        <i class="fas fa-sign-out-alt"></i>Logout
                    </button>
                </div>
            </div>
        </div>

        <div class="customer-main">
            <div class="customer-sidebar">
                <ul class="sidebar-menu">
                    <li><a href="#" class="active" data-section="customer-articles">
                        <i class="fas fa-newspaper"></i>Articles
                    </a></li>
                    <li><a href="#" data-section="customer-blogs">
                        <i class="fas fa-blog"></i>Blog Posts
                    </a></li>
                    <li><a href="#" data-section="customer-literature">
                        <i class="fas fa-book"></i>Literature
                    </a></li>
                    <li><a href="#" data-section="customer-messaging">
                        <i class="fas fa-comments"></i>Messaging
                    </a></li>
                    <li><a href="#" data-section="customer-profile">
                        <i class="fas fa-user"></i>My Profile
                    </a></li>
                </ul>
            </div>

            <div class="admin-content">
                <!-- Customer Articles Section -->
                <div class="customer-section active" id="customer-articles-section">
                    <h2 style="margin-bottom: 3rem;">Articles</h2>
                    <div class="articles-grid" id="customer-articles-container">
                        <div class="empty-state">
                            <i class="fas fa-newspaper"></i>
                            <h3>No Articles Available</h3>
                            <p>Check back later for new articles.</p>
                        </div>
                    </div>
                </div>

                <!-- Customer Blogs Section -->
                <div class="customer-section" id="customer-blogs-section">
                    <h2 style="margin-bottom: 3rem;">Blog Posts</h2>
                    <div class="blog-grid" id="customer-blogs-container">
                        <div class="empty-state">
                            <i class="fas fa-blog"></i>
                            <h3>No Blog Posts Available</h3>
                            <p>Check back later for new blog posts.</p>
                        </div>
                    </div>
                </div>

                <!-- Customer Literature Section -->
                <div class="customer-section" id="customer-literature-section">
                    <h2 style="margin-bottom: 3rem;">Literature</h2>
                    <div class="articles-grid" id="customer-literature-container">
                        <div class="empty-state">
                            <i class="fas fa-book"></i>
                            <h3>No Literature Available</h3>
                            <p>Check back later for new literature pieces.</p>
                        </div>
                    </div>
                </div>

                <!-- Customer Messaging Section -->
                <div class="customer-section" id="customer-messaging-section">
                    <h2 style="margin-bottom: 3rem;">Messaging</h2>
                    
                    <div class="card" style="padding: 2.5rem;">
                        <div class="messages-container" id="customer-messages-container">
                            <div class="empty-state">
                                <i class="fas fa-comments"></i>
                                <h3>No Messages</h3>
                                <p>Start a conversation with the admin!</p>
                            </div>
                        </div>
                        
                        <form class="message-form" id="customer-message-form">
                            <input type="text" class="message-input" id="customer-message-input" placeholder="Type your message to admin..." required>
                            <button type="submit" class="btn btn-success">
                                <i class="fas fa-paper-plane"></i>Send
                            </button>
                        </form>
                    </div>
                </div>

                <!-- Customer Profile Section -->
                <div class="customer-section" id="customer-profile-section">
                    <h2 style="margin-bottom: 3rem;">My Profile</h2>
                    <div class="card" style="padding: 2.5rem;">
                        <div id="customer-profile-content">
                            <!-- Customer profile will be loaded here -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Login Modal -->
    <div class="modal" id="login-modal">
        <div class="modal-content" style="max-width: 500px; padding: 3rem;">
            <button class="modal-close" onclick="closeModal('login-modal')">
                <i class="fas fa-times"></i>
            </button>
            <div style="text-align: center; margin-bottom: 2.5rem;">
                <i class="fas fa-lock" style="font-size: 3.5rem; color: var(--accent); margin-bottom: 1.5rem;"></i>
                <h2 style="margin-bottom: 0.75rem;">Account Access</h2>
                <p class="text-muted">Choose your account type and enter credentials</p>
            </div>
            
            <div class="auth-tabs">
                <button class="auth-tab active" data-tab="admin-login">Admin Login</button>
                <button class="auth-tab" data-tab="customer-login">Customer Login</button>
                <button class="auth-tab" data-tab="customer-register">Customer Register</button>
            </div>
            
            <!-- Admin Login Form -->
            <form id="admin-login-form" class="auth-form active">
                <div class="form-group">
                    <label class="form-label">Email</label>
                    <input type="email" class="form-control" id="admin-login-email" placeholder="Enter admin email" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Password</label>
                    <input type="password" class="form-control" id="admin-login-password" placeholder="Enter your password" required>
                </div>
                <button type="submit" class="btn btn-success btn-block" style="margin-top: 1.5rem;">
                    <i class="fas fa-sign-in-alt"></i>Sign In to Admin Dashboard
                </button>
                <div class="text-center mt-3">
                    <p class="text-muted">Default Human Credentials:</p>
                    <p><strong>Email:</strong> ----</p>
                    <p><strong>Password:</strong>----</p>
                </div>
            </form>
            
            <!-- Customer Login Form -->
            <form id="customer-login-form" class="auth-form">
                <div class="form-group">
                    <label class="form-label">Email</label>
                    <input type="email" class="form-control" id="customer-login-email" placeholder="Enter your email" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Password</label>
                    <input type="password" class="form-control" id="customer-login-password" placeholder="Enter your password" required>
                </div>
                <button type="submit" class="btn btn-success btn-block" style="margin-top: 1.5rem;">
                    <i class="fas fa-sign-in-alt"></i>Sign In to Customer Dashboard
                </button>
            </form>
            
            <!-- Customer Register Form -->
            <form id="customer-register-form" class="auth-form">
                <div class="form-group">
                    <label class="form-label">Full Name</label>
                    <input type="text" class="form-control" id="customer-register-name" placeholder="Enter your full name" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Father Name</label>
                    <input type="text" class="form-control" id="customer-register-father" placeholder="Enter father's name" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Phone Number</label>
                    <input type="tel" class="form-control" id="customer-register-phone" placeholder="Enter phone number" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Address</label>
                    <textarea class="form-control" id="customer-register-address" placeholder="Enter your address" rows="2" required></textarea>
                </div>
                <div class="form-group">
                    <label class="form-label">Description</label>
                    <textarea class="form-control" id="customer-register-description" placeholder="Tell us about yourself" rows="3"></textarea>
                </div>
                <div class="form-group">
                    <label class="form-label">Email</label>
                    <input type="email" class="form-control" id="customer-register-email" placeholder="Enter your email" required>
                </div>
                <div class="form-group">
                    <label class="form-label">Password</label>
                    <input type="password" class="form-control" id="customer-register-password" placeholder="Create a password (min 6 characters)" required minlength="6">
                </div>
                <div class="form-group">
                    <label class="form-label">Confirm Password</label>
                    <input type="password" class="form-control" id="customer-register-confirm-password" placeholder="Confirm your password" required>
                </div>
                <button type="submit" class="btn btn-success btn-block" style="margin-top: 1.5rem;">
                    <i class="fas fa-user-plus"></i>Create Customer Account
                </button>
            </form>
            
            <div style="margin-top: 2rem; padding: 1.5rem; background: rgba(233, 69, 96, 0.05); border-radius: var(--border-radius);">
                <h4 style="margin-bottom: 0.5rem; color: var(--accent);">Samad Daud</h4>
                <p style="margin: 0; font-size: 0.9rem;"><strong>Occupation:</strong> Writer</p>
                <p style="margin: 0; font-size: 0.9rem;"><strong>Contact No:</strong> +92 324 7736384</p>
            </div>
        </div>
    </div>

    <!-- Article View Modal -->
    <div class="modal" id="article-modal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('article-modal')">
                <i class="fas fa-times"></i>
            </button>
            <div id="article-modal-content" style="padding: 3rem;">
                <!-- Article content will be loaded here dynamically -->
            </div>
        </div>
    </div>

    <!-- Blog View Modal -->
    <div class="modal" id="blog-modal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('blog-modal')">
                <i class="fas fa-times"></i>
            </button>
            <div id="blog-modal-content" style="padding: 3rem;">
                <!-- Blog content will be loaded here dynamically -->
            </div>
        </div>
    </div>

    <!-- Literature View Modal -->
    <div class="modal" id="literature-modal">
        <div class="modal-content">
            <button class="modal-close" onclick="closeModal('literature-modal')">
                <i class="fas fa-times"></i>
            </button>
            <div id="literature-modal-content" style="padding: 3rem;">
                <!-- Literature content will be loaded here dynamically -->
            </div>
        </div>
    </div>

    <!-- Dark Mode Toggle -->
    <div class="dark-mode-toggle">
        <i class="fas fa-moon" id="dark-mode-icon"></i>
    </div>

    <script>
        // Global Variables
        let currentUser = null;
        let currentUserRole = null;
        let articles = [];
        let blogs = [];
        let literature = [];
        let customers = [];
        let contentWritingProjects = [];
        let mediaFiles = [];
        let settings = {};
        let comments = {};
        let messages = [];
        let conversations = [];
        let categories = ['philosophy', 'psychology', 'creativity', 'mindfulness', 'literature', 'science'];
        let editingArticleId = null;
        let editingBlogId = null;
        let editingLiteratureId = null;
        let editingCustomerId = null;
        let currentConversationId = null;

        // DOM Elements
        const elements = {
            adminLoginBtn: document.getElementById('admin-login-btn'),
            customerLoginBtn: document.getElementById('customer-login-btn'),
            loginModal: document.getElementById('login-modal'),
            adminPanel: document.getElementById('admin-panel'),
            customerPanel: document.getElementById('customer-panel'),
            adminLoginForm: document.getElementById('admin-login-form'),
            customerLoginForm: document.getElementById('customer-login-form'),
            customerRegisterForm: document.getElementById('customer-register-form'),
            adminLogoutBtn: document.getElementById('admin-logout'),
            customerLogoutBtn: document.getElementById('customer-logout'),
            adminUserEmail: document.getElementById('admin-user-email'),
            customerUserName: document.getElementById('customer-user-name'),
            sidebarLinks: document.querySelectorAll('.sidebar-menu a'),
            adminSections: document.querySelectorAll('.admin-section'),
            customerSections: document.querySelectorAll('.customer-section'),
            addArticleBtn: document.getElementById('add-article-btn'),
            addBlogBtn: document.getElementById('add-blog-btn'),
            addLiteratureBtn: document.getElementById('add-literature-btn'),
            addCustomerBtn: document.getElementById('add-customer-btn'),
            cancelArticleBtn: document.getElementById('cancel-article-btn'),
            cancelBlogBtn: document.getElementById('cancel-blog-btn'),
            cancelLiteratureBtn: document.getElementById('cancel-literature-btn'),
            articlesSection: document.getElementById('articles-section'),
            blogSection: document.getElementById('blog-section'),
            literatureSection: document.getElementById('literature-section'),
            customersSection: document.getElementById('customers-section'),
            settingsSection: document.getElementById('settings-section'),
            addArticleSection: document.getElementById('add-article-section'),
            addBlogSection: document.getElementById('add-blog-section'),
            addLiteratureSection: document.getElementById('add-literature-section'),
            articleForm: document.getElementById('article-form'),
            blogForm: document.getElementById('blog-form'),
            literatureForm: document.getElementById('literature-form'),
            settingsForm: document.getElementById('settings-form'),
            contentWritingForm: document.getElementById('content-writing-form'),
            featuredImageInput: document.getElementById('featured-image'),
            blogFeaturedImageInput: document.getElementById('blog-featured-image'),
            literatureCoverImageInput: document.getElementById('literature-cover-image'),
            mediaFilesInput: document.getElementById('media-files'),
            uploadProgressContainer: document.getElementById('upload-progress-container'),
            uploadProgress: document.getElementById('upload-progress'),
            uploadStatus: document.getElementById('upload-status'),
            featuredArticlesContainer: document.getElementById('featured-articles-container'),
            blogContainer: document.getElementById('blog-container'),
            literatureContainer: document.getElementById('literature-container'),
            customerArticlesContainer: document.getElementById('customer-articles-container'),
            customerBlogsContainer: document.getElementById('customer-blogs-container'),
            customerLiteratureContainer: document.getElementById('customer-literature-container'),
            customerProfileContent: document.getElementById('customer-profile-content'),
            articlesTableBody: document.getElementById('articles-table-body'),
            blogTableBody: document.getElementById('blog-table-body'),
            literatureTableBody: document.getElementById('literature-table-body'),
            customersTableBody: document.getElementById('customers-table-body'),
            contentWritingTableBody: document.getElementById('content-writing-table-body'),
            mediaGallery: document.getElementById('media-gallery'),
            publicMediaGallery: document.getElementById('public-media-gallery'),
            videoContainer: document.getElementById('video-container'),
            articleSearch: document.getElementById('article-search'),
            emptyStateCreateBtn: document.getElementById('empty-state-create-btn'),
            emptyStateCreateBlogBtn: document.getElementById('empty-state-create-blog-btn'),
            emptyStateCreateLiteratureBtn: document.getElementById('empty-state-create-literature-btn'),
            emptyStateCreateContentBtn: document.getElementById('empty-state-create-content-btn'),
            darkModeIcon: document.getElementById('dark-mode-icon'),
            contactTitle: document.getElementById('contact-title'),
            contactAddress: document.getElementById('contact-address'),
            contactEmail: document.getElementById('contact-email'),
            contactPhone: document.getElementById('contact-phone'),
            contactOwner: document.getElementById('contact-owner'),
            contactDescription: document.getElementById('contact-description'),
            youtubeLink: document.getElementById('youtube-link'),
            whatsappLink: document.getElementById('whatsapp-link'),
            facebookLink: document.getElementById('facebook-link'),
            adminMessageForm: document.getElementById('admin-message-form'),
            adminMessageInput: document.getElementById('admin-message-input'),
            adminMessagesContainer: document.getElementById('admin-messages-container'),
            adminConversationsList: document.getElementById('admin-conversations-list'),
            adminMessagesHeader: document.getElementById('admin-messages-header'),
            customerMessageForm: document.getElementById('customer-message-form'),
            customerMessageInput: document.getElementById('customer-message-input'),
            customerMessagesContainer: document.getElementById('customer-messages-container')
        };

        // Initialize Application
        document.addEventListener('DOMContentLoaded', function() {
            initializeApp();
            setupEventListeners();
            setupFirebaseAuthListener();
        });

        function initializeApp() {
            loadFromFirebase();
            updateDisplay();
            setupSearchFunctionality();
            initializeDarkMode();
            loadSettings();
        }

        function setupEventListeners() {
            // Login functionality
            elements.adminLoginBtn.addEventListener('click', (e) => {
                e.preventDefault();
                elements.loginModal.classList.add('active');
                switchAuthTab('admin-login');
            });

            elements.customerLoginBtn.addEventListener('click', (e) => {
                e.preventDefault();
                elements.loginModal.classList.add('active');
                switchAuthTab('customer-login');
            });

            elements.adminLoginForm.addEventListener('submit', handleAdminLogin);
            elements.customerLoginForm.addEventListener('submit', handleCustomerLogin);
            elements.customerRegisterForm.addEventListener('submit', handleCustomerRegister);
            elements.adminLogoutBtn.addEventListener('click', handleAdminLogout);
            elements.customerLogoutBtn.addEventListener('click', handleCustomerLogout);

            // Auth tabs
            document.querySelectorAll('.auth-tab').forEach(tab => {
                tab.addEventListener('click', (e) => {
                    const tabName = e.target.getAttribute('data-tab');
                    switchAuthTab(tabName);
                });
            });

            // Article management
            elements.addArticleBtn.addEventListener('click', showAddArticleForm);
            elements.cancelArticleBtn.addEventListener('click', showArticlesSection);
            elements.articleForm.addEventListener('submit', handleArticleSubmit);
            elements.emptyStateCreateBtn.addEventListener('click', showAddArticleForm);

            // Blog management
            elements.addBlogBtn.addEventListener('click', showAddBlogForm);
            elements.cancelBlogBtn.addEventListener('click', showBlogSection);
            elements.blogForm.addEventListener('submit', handleBlogSubmit);
            elements.emptyStateCreateBlogBtn.addEventListener('click', showAddBlogForm);

            // Literature management
            elements.addLiteratureBtn.addEventListener('click', showAddLiteratureForm);
            elements.cancelLiteratureBtn.addEventListener('click', showLiteratureSection);
            elements.literatureForm.addEventListener('submit', handleLiteratureSubmit);
            elements.emptyStateCreateLiteratureBtn.addEventListener('click', showAddLiteratureForm);

            // Content writing
            elements.contentWritingForm.addEventListener('submit', handleContentWritingSubmit);
            elements.emptyStateCreateContentBtn.addEventListener('click', () => {
                showAdminSection('content-writing-section');
            });

            // Settings
            elements.settingsForm.addEventListener('submit', handleSettingsSubmit);

            // Media upload
            elements.featuredImageInput.addEventListener('change', handleFeaturedImageUpload);
            elements.blogFeaturedImageInput.addEventListener('change', handleBlogFeaturedImageUpload);
            elements.literatureCoverImageInput.addEventListener('change', handleLiteratureCoverImageUpload);
            elements.mediaFilesInput.addEventListener('change', handleMediaUpload);

            // Messaging
            elements.adminMessageForm.addEventListener('submit', handleAdminMessageSubmit);
            elements.customerMessageForm.addEventListener('submit', handleCustomerMessageSubmit);

            // Navigation
            elements.sidebarLinks.forEach(link => {
                link.addEventListener('click', handleSidebarNavigation);
            });

            // Mobile menu
            document.querySelector('.mobile-toggle').addEventListener('click', toggleMobileMenu);

            // Dark mode
            document.querySelector('.dark-mode-toggle').addEventListener('click', toggleDarkMode);

            // Close modals when clicking outside
            document.addEventListener('click', handleOutsideClick);
        }

        function setupFirebaseAuthListener() {
            if (window.firebaseOnAuthStateChanged && window.firebaseAuth) {
                window.firebaseOnAuthStateChanged(window.firebaseAuth, (user) => {
                    if (user) {
                        // User is signed in
                        currentUser = user;
                        
                        // Check user role
                        checkUserRole(user.uid).then(role => {
                            currentUserRole = role;
                            
                            if (role === 'admin') {
                                elements.adminPanel.style.display = 'block';
                                elements.customerPanel.style.display = 'none';
                                document.body.style.overflow = 'hidden';
                                elements.adminUserEmail.textContent = `Welcome, ${user.email}`;
                                showNotification('Welcome back, Admin!', 'success');
                                
                                // Load data from Firebase
                                loadFromFirebase();
                            } else if (role === 'customer') {
                                elements.adminPanel.style.display = 'none';
                                elements.customerPanel.style.display = 'block';
                                document.body.style.overflow = 'hidden';
                                loadCustomerProfile(user.uid);
                                showNotification('Welcome to your dashboard!', 'success');
                                
                                // Load public content for customers
                                loadPublicContent();
                                loadCustomerMessages();
                            }
                        });
                    } else {
                        // User is signed out
                        currentUser = null;
                        currentUserRole = null;
                        elements.adminPanel.style.display = 'none';
                        elements.customerPanel.style.display = 'none';
                        document.body.style.overflow = 'auto';
                    }
                });
            } else {
                console.error('Firebase Auth not available');
            }
        }

        async function checkUserRole(userId) {
            // For demo purposes, check if user is the default admin
            if (currentUser && currentUser.email === "admin@thoughtspower.com") {
                return 'admin';
            }
            
            if (!window.firebaseDBGet || !window.firebaseDBRef || !window.firebaseDB) {
                return 'customer';
            }
            
            try {
                // Check if user is in admin collection
                const adminRef = window.firebaseDBRef(window.firebaseDB, `admins/${userId}`);
                const adminSnapshot = await window.firebaseDBGet(adminRef);
                
                if (adminSnapshot.exists()) {
                    return 'admin';
                }
                
                // Check if user is in customer collection
                const customerRef = window.firebaseDBRef(window.firebaseDB, `customers/${userId}`);
                const customerSnapshot = await window.firebaseDBGet(customerRef);
                
                if (customerSnapshot.exists()) {
                    return 'customer';
                }
                
                // Default to customer if not found in either
                return 'customer';
            } catch (error) {
                console.error('Error checking user role:', error);
                return 'customer';
            }
        }

        function switchAuthTab(tabName) {
            // Update tabs
            document.querySelectorAll('.auth-tab').forEach(tab => {
                tab.classList.toggle('active', tab.getAttribute('data-tab') === tabName);
            });
            
            // Update forms
            document.querySelectorAll('.auth-form').forEach(form => {
                form.classList.toggle('active', form.id === `${tabName}-form`);
            });
        }

        function initializeDarkMode() {
            const darkMode = localStorage.getItem('darkMode') === 'true';
            const darkModeIcon = localStorage.getItem('darkModeIcon') || 'moon';
            
            if (darkMode) {
                document.body.classList.add('dark-mode');
                elements.darkModeIcon.className = darkModeIcon === 'sun' ? 'fas fa-sun' : 'fas fa-moon';
            } else {
                elements.darkModeIcon.className = 'fas fa-moon';
            }
        }

        async function handleAdminLogin(e) {
            e.preventDefault();
            const email = document.getElementById('admin-login-email').value;
            const password = document.getElementById('admin-login-password').value;
            
            if (!window.firebaseSignIn || !window.firebaseAuth) {
                showNotification('Firebase authentication not available', 'error');
                return;
            }
            
            try {
                const userCredential = await window.firebaseSignIn(window.firebaseAuth, email, password);
                const user = userCredential.user;
                
                // Success - Firebase auth listener will handle the rest
                closeModal('login-modal');
                elements.adminLoginForm.reset();
                showNotification('Admin login successful!', 'success');
            } catch (error) {
                console.error("Admin sign in failed:", error);
                showNotification('Login failed: ' + error.message, 'error');
            }
        }

        async function handleCustomerLogin(e) {
            e.preventDefault();
            const email = document.getElementById('customer-login-email').value;
            const password = document.getElementById('customer-login-password').value;
            
            if (!window.firebaseSignIn || !window.firebaseAuth) {
                showNotification('Firebase authentication not available', 'error');
                return;
            }
            
            try {
                const userCredential = await window.firebaseSignIn(window.firebaseAuth, email, password);
                const user = userCredential.user;
                
                // Success - Firebase auth listener will handle the rest
                closeModal('login-modal');
                elements.customerLoginForm.reset();
            } catch (error) {
                console.error("Customer sign in failed:", error);
                showNotification('Login failed: ' + error.message, 'error');
            }
        }

        async function handleCustomerRegister(e) {
            e.preventDefault();
            const name = document.getElementById('customer-register-name').value;
            const fatherName = document.getElementById('customer-register-father').value;
            const phone = document.getElementById('customer-register-phone').value;
            const address = document.getElementById('customer-register-address').value;
            const description = document.getElementById('customer-register-description').value;
            const email = document.getElementById('customer-register-email').value;
            const password = document.getElementById('customer-register-password').value;
            const confirmPassword = document.getElementById('customer-register-confirm-password').value;
            
            if (password !== confirmPassword) {
                showNotification('Passwords do not match', 'error');
                return;
            }
            
            if (!window.firebaseCreateUser || !window.firebaseAuth) {
                showNotification('Firebase authentication not available', 'error');
                return;
            }
            
            try {
                const userCredential = await window.firebaseCreateUser(window.firebaseAuth, email, password);
                const user = userCredential.user;
                
                // Save customer profile to database
                await saveCustomerProfile(user.uid, {
                    name: name,
                    fatherName: fatherName,
                    phone: phone,
                    address: address,
                    description: description,
                    email: email,
                    createdAt: new Date().toISOString(),
                    role: 'customer'
                });
                
                showNotification('Account created successfully! You can now login.', 'success');
                switchAuthTab('customer-login');
                elements.customerRegisterForm.reset();
            } catch (error) {
                console.error("Customer registration failed:", error);
                showNotification('Registration failed: ' + error.message, 'error');
            }
        }

        async function saveCustomerProfile(userId, customerData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const customerRef = window.firebaseDBRef(window.firebaseDB, `customers/${userId}`);
            await window.firebaseDBSet(customerRef, customerData);
        }

        async function handleAdminLogout() {
            if (window.firebaseSignOut && window.firebaseAuth) {
                try {
                    await window.firebaseSignOut(window.firebaseAuth);
                    showNotification('Logged out successfully', 'success');
                } catch (error) {
                    console.error("Admin sign out failed:", error);
                    showNotification('Logout failed: ' + error.message, 'error');
                }
            }
        }

        async function handleCustomerLogout() {
            if (window.firebaseSignOut && window.firebaseAuth) {
                try {
                    await window.firebaseSignOut(window.firebaseAuth);
                    showNotification('Logged out successfully', 'success');
                } catch (error) {
                    console.error("Customer sign out failed:", error);
                    showNotification('Logout failed: ' + error.message, 'error');
                }
            }
        }

        function handleSidebarNavigation(e) {
            e.preventDefault();
            const section = e.currentTarget.getAttribute('data-section');
            
            if (currentUserRole === 'admin') {
                showAdminSection(section + '-section');
            } else if (currentUserRole === 'customer') {
                showCustomerSection(section + '-section');
            }
            
            // Update active state
            const parentMenu = e.currentTarget.closest('.sidebar-menu');
            parentMenu.querySelectorAll('a').forEach(l => l.classList.remove('active'));
            e.currentTarget.classList.add('active');
        }

        async function handleArticleSubmit(e) {
            e.preventDefault();
            await saveArticle();
        }

        async function handleBlogSubmit(e) {
            e.preventDefault();
            await saveBlog();
        }

        async function handleLiteratureSubmit(e) {
            e.preventDefault();
            await saveLiterature();
        }

        async function handleContentWritingSubmit(e) {
            e.preventDefault();
            await saveContentWritingProject();
        }

        async function handleSettingsSubmit(e) {
            e.preventDefault();
            await saveSettings();
        }

        async function handleAdminMessageSubmit(e) {
            e.preventDefault();
            await sendAdminMessage();
        }

        async function handleCustomerMessageSubmit(e) {
            e.preventDefault();
            await sendCustomerMessage();
        }

        function handleFeaturedImageUpload(e) {
            const file = e.target.files[0];
            if (file && file.type.startsWith('image/')) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const preview = document.getElementById('featured-image-preview');
                    preview.innerHTML = `
                        <div class="preview-item">
                            <img src="${e.target.result}" alt="Featured image preview for article">
                            <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                                <i class="fas fa-times"></i>
                            </button>
                        </div>
                    `;
                };
                reader.readAsDataURL(file);
            }
        }

        function handleBlogFeaturedImageUpload(e) {
            const file = e.target.files[0];
            if (file && file.type.startsWith('image/')) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const preview = document.getElementById('blog-featured-image-preview');
                    preview.innerHTML = `
                        <div class="preview-item">
                            <img src="${e.target.result}" alt="Featured image preview for blog">
                            <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                                <i class="fas fa-times"></i>
                            </button>
                        </div>
                    `;
                };
                reader.readAsDataURL(file);
            }
        }

        function handleLiteratureCoverImageUpload(e) {
            const file = e.target.files[0];
            if (file && file.type.startsWith('image/')) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    const preview = document.getElementById('literature-cover-image-preview');
                    preview.innerHTML = `
                        <div class="preview-item">
                            <img src="${e.target.result}" alt="Cover image preview for literature">
                            <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                                <i class="fas fa-times"></i>
                            </button>
                        </div>
                    `;
                };
                reader.readAsDataURL(file);
            }
        }

        async function handleMediaUpload(e) {
            const files = e.target.files;
            if (!files || files.length === 0) return;
            
            // Show progress container
            elements.uploadProgressContainer.style.display = 'block';
            elements.uploadProgress.style.width = '0%';
            elements.uploadStatus.textContent = 'Starting upload...';
            
            const uploadPromises = [];
            
            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                
                // Validate file type
                if (!file.type.startsWith('image/') && !file.type.startsWith('video/')) {
                    console.warn("Skipping unsupported file:", file.name);
                    continue;
                }
                
                // Validate file size (100MB limit)
                if (file.size > 100 * 1024 * 1024) {
                    showNotification(`${file.name} is too large (max 100MB)`, 'error');
                    continue;
                }
                
                // Generate unique filename
                const timestamp = Date.now();
                const randomString = Math.random().toString(36).substring(2, 15);
                const fileExtension = file.name.split('.').pop();
                const uniqueName = `${timestamp}_${randomString}.${fileExtension}`;
                
                // Create storage reference
                const storageRef = window.firebaseStorageRef(window.firebaseStorage, `media/${uniqueName}`);
                
                // Create upload task
                const uploadTask = window.firebaseUploadBytes(storageRef, file);
                
                // Wrap upload in a promise
                const uploadPromise = new Promise((resolve, reject) => {
                    uploadTask.on('state_changed',
                        (snapshot) => {
                            // Update progress
                            const progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
                            elements.uploadProgress.style.width = `${progress}%`;
                            elements.uploadStatus.textContent = `Uploading ${file.name}: ${Math.round(progress)}%`;
                        },
                        (error) => {
                            reject(error);
                        },
                        async () => {
                            // Upload completed successfully
                            try {
                                const downloadURL = await window.firebaseGetDownloadURL(uploadTask.snapshot.ref);
                                
                                const mediaItem = {
                                    id: 'media-' + timestamp + '-' + randomString,
                                    url: downloadURL,
                                    name: file.name,
                                    size: file.size,
                                    type: file.type,
                                    uploadedAt: new Date().toISOString(),
                                    storagePath: `media/${uniqueName}`,
                                    uploadedBy: currentUser.uid
                                };
                                
                                resolve(mediaItem);
                            } catch (error) {
                                reject(error);
                            }
                        }
                    );
                });
                
                uploadPromises.push(uploadPromise);
            }
            
            try {
                const uploadedFiles = await Promise.all(uploadPromises);
                
                // Save to Firebase Database
                for (const mediaItem of uploadedFiles) {
                    await saveMediaToFirebase(mediaItem);
                }
                
                showNotification(`Successfully uploaded ${uploadedFiles.length} files`, 'success');
                
                // Reset progress
                elements.uploadProgressContainer.style.display = 'none';
                elements.mediaFilesInput.value = '';
                
                // Reload media from Firebase
                loadMediaFromFirebase();
                
            } catch (error) {
                console.error('Upload failed:', error);
                showNotification('Upload failed: ' + error.message, 'error');
                elements.uploadProgressContainer.style.display = 'none';
            }
        }

        function handleOutsideClick(e) {
            // Close mobile menu when clicking outside
            if (!e.target.closest('.nav-menu') && !e.target.closest('.mobile-toggle')) {
                document.querySelector('.nav-menu').classList.remove('active');
            }
            
            // Close modals when clicking outside content
            if (e.target.classList.contains('modal')) {
                closeModal(e.target.id);
            }
        }

        function setupSearchFunctionality() {
            elements.articleSearch.addEventListener('input', (e) => {
                const searchTerm = e.target.value.toLowerCase();
                filterArticles(searchTerm);
            });
        }

        function filterArticles(searchTerm) {
            const filteredArticles = articles.filter(article => 
                article.title.toLowerCase().includes(searchTerm) ||
                article.content.toLowerCase().includes(searchTerm) ||
                article.category.toLowerCase().includes(searchTerm) ||
                (article.tags && article.tags.some(tag => tag.toLowerCase().includes(searchTerm)))
            );
            
            updateArticlesDisplay(filteredArticles);
        }

        function showAdminSection(sectionId) {
            elements.adminSections.forEach(section => {
                section.classList.remove('active');
            });
            document.getElementById(sectionId).classList.add('active');
        }

        function showCustomerSection(sectionId) {
            elements.customerSections.forEach(section => {
                section.classList.remove('active');
            });
            document.getElementById(sectionId).classList.add('active');
        }

        function showAddArticleForm() {
            editingArticleId = null;
            showAdminSection('add-article-section');
            document.getElementById('article-form-title').textContent = 'Create New Article';
            document.getElementById('article-id').value = '';
            elements.articleForm.reset();
            document.getElementById('featured-image-preview').innerHTML = '';
        }

        function showAddBlogForm() {
            editingBlogId = null;
            showAdminSection('add-blog-section');
            document.getElementById('blog-form-title').textContent = 'Create New Blog Post';
            document.getElementById('blog-id').value = '';
            elements.blogForm.reset();
            document.getElementById('blog-featured-image-preview').innerHTML = '';
        }

        function showAddLiteratureForm() {
            editingLiteratureId = null;
            showAdminSection('add-literature-section');
            document.getElementById('literature-form-title').textContent = 'Add New Literature';
            document.getElementById('literature-id').value = '';
            elements.literatureForm.reset();
            document.getElementById('literature-cover-image-preview').innerHTML = '';
        }

        function showArticlesSection() {
            showAdminSection('articles-section');
        }

        function showBlogSection() {
            showAdminSection('blog-section');
        }

        function showLiteratureSection() {
            showAdminSection('literature-section');
        }

        function showMediaSection() {
            showAdminSection('media-section');
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
        }

        function toggleMobileMenu() {
            document.querySelector('.nav-menu').classList.toggle('active');
        }

        function toggleDarkMode() {
            const isDarkMode = document.body.classList.toggle('dark-mode');
            const icon = elements.darkModeIcon;
            
            if (isDarkMode) {
                icon.className = 'fas fa-sun';
                localStorage.setItem('darkModeIcon', 'sun');
            } else {
                icon.className = 'fas fa-moon';
                localStorage.setItem('darkModeIcon', 'moon');
            }
            
            localStorage.setItem('darkMode', isDarkMode);
        }

        async function saveArticle() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to save articles', 'error');
                return;
            }
            
            const articleId = document.getElementById('article-id').value;
            const articleData = {
                title: document.getElementById('article-title').value,
                category: document.getElementById('article-category').value,
                content: document.getElementById('article-content').value,
                tags: document.getElementById('article-tags').value ? 
                      document.getElementById('article-tags').value.split(',').map(tag => tag.trim()) : [],
                featuredImage: document.getElementById('featured-image-preview').querySelector('img')?.src || 
                              '/images/default-article.jpg',
                createdAt: articleId ? articles.find(a => a.id === articleId).createdAt : new Date().toISOString(),
                updatedAt: new Date().toISOString(),
                status: 'published',
                views: articleId ? articles.find(a => a.id === articleId).views || 0 : 0,
                likes: articleId ? articles.find(a => a.id === articleId).likes || 0 : 0,
                author: currentUser.email,
                userId: currentUser.uid
            };

            try {
                if (articleId) {
                    // Update existing article
                    await updateArticleInFirebase(articleId, articleData);
                    showNotification('Article updated successfully!', 'success');
                } else {
                    // Create new article
                    articleData.id = 'article-' + Date.now();
                    articleData.views = 0;
                    articleData.likes = 0;
                    await saveArticleToFirebase(articleData);
                    showNotification('Article published successfully!', 'success');
                }

                await loadArticlesFromFirebase();
                updateDisplay();
                showAdminSection('articles-section');
            } catch (error) {
                console.error('Error saving article:', error);
                showNotification('Failed to save article: ' + error.message, 'error');
            }
        }

        async function saveBlog() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to save blog posts', 'error');
                return;
            }
            
            const blogId = document.getElementById('blog-id').value;
            const blogData = {
                title: document.getElementById('blog-title').value,
                category: document.getElementById('blog-category').value,
                content: document.getElementById('blog-content').value,
                tags: document.getElementById('blog-tags').value ? 
                      document.getElementById('blog-tags').value.split(',').map(tag => tag.trim()) : [],
                featuredImage: document.getElementById('blog-featured-image-preview').querySelector('img')?.src || 
                              '/images/default-blog.jpg',
                createdAt: blogId ? blogs.find(b => b.id === blogId).createdAt : new Date().toISOString(),
                updatedAt: new Date().toISOString(),
                status: 'published',
                views: blogId ? blogs.find(b => b.id === blogId).views || 0 : 0,
                likes: blogId ? blogs.find(b => b.id === blogId).likes || 0 : 0,
                author: currentUser.email,
                userId: currentUser.uid
            };

            try {
                if (blogId) {
                    // Update existing blog
                    await updateBlogInFirebase(blogId, blogData);
                    showNotification('Blog post updated successfully!', 'success');
                } else {
                    // Create new blog
                    blogData.id = 'blog-' + Date.now();
                    blogData.views = 0;
                    blogData.likes = 0;
                    await saveBlogToFirebase(blogData);
                    showNotification('Blog post published successfully!', 'success');
                }

                await loadBlogsFromFirebase();
                updateDisplay();
                showAdminSection('blog-section');
            } catch (error) {
                console.error('Error saving blog:', error);
                showNotification('Failed to save blog: ' + error.message, 'error');
            }
        }

        async function saveLiterature() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to save literature', 'error');
                return;
            }
            
            const literatureId = document.getElementById('literature-id').value;
            const literatureData = {
                title: document.getElementById('literature-title').value,
                type: document.getElementById('literature-type').value,
                author: document.getElementById('literature-author').value,
                year: document.getElementById('literature-year').value,
                content: document.getElementById('literature-content').value,
                summary: document.getElementById('literature-summary').value,
                coverImage: document.getElementById('literature-cover-image-preview').querySelector('img')?.src || 
                           '/images/default-literature.jpg',
                createdAt: literatureId ? literature.find(l => l.id === literatureId).createdAt : new Date().toISOString(),
                updatedAt: new Date().toISOString(),
                status: 'published',
                views: literatureId ? literature.find(l => l.id === literatureId).views || 0 : 0,
                likes: literatureId ? literature.find(l => l.id === literatureId).likes || 0 : 0,
                userId: currentUser.uid
            };

            try {
                if (literatureId) {
                    // Update existing literature
                    await updateLiteratureInFirebase(literatureId, literatureData);
                    showNotification('Literature updated successfully!', 'success');
                } else {
                    // Create new literature
                    literatureData.id = 'literature-' + Date.now();
                    literatureData.views = 0;
                    literatureData.likes = 0;
                    await saveLiteratureToFirebase(literatureData);
                    showNotification('Literature added successfully!', 'success');
                }

                await loadLiteratureFromFirebase();
                updateDisplay();
                showAdminSection('literature-section');
            } catch (error) {
                console.error('Error saving literature:', error);
                showNotification('Failed to save literature: ' + error.message, 'error');
            }
        }

        async function saveContentWritingProject() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to create projects', 'error');
                return;
            }
            
            const projectData = {
                name: document.getElementById('project-name').value,
                client: document.getElementById('client-name').value,
                type: document.getElementById('content-type').value,
                deadline: document.getElementById('project-deadline').value,
                description: document.getElementById('project-description').value,
                createdAt: new Date().toISOString(),
                status: 'active',
                createdBy: currentUser.uid
            };

            try {
                projectData.id = 'project-' + Date.now();
                await saveContentWritingProjectToFirebase(projectData);
                
                showNotification('Content writing project created successfully!', 'success');
                elements.contentWritingForm.reset();
                await loadContentWritingProjectsFromFirebase();
            } catch (error) {
                console.error('Error saving project:', error);
                showNotification('Failed to create project: ' + error.message, 'error');
            }
        }

        async function saveSettings() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to save settings', 'error');
                return;
            }
            
            const settingsData = {
                title: document.getElementById('store-title').value,
                address: document.getElementById('store-address').value,
                email: document.getElementById('store-email').value,
                phone: document.getElementById('store-phone').value,
                owner: document.getElementById('owner-name').value,
                description: document.getElementById('store-description').value,
                youtube: document.getElementById('youtube-channel').value,
                whatsapp: document.getElementById('whatsapp-channel').value,
                facebook: document.getElementById('facebook-page').value,
                updatedAt: new Date().toISOString(),
                updatedBy: currentUser.uid
            };

            try {
                await saveSettingsToFirebase(settingsData);
                showNotification('Settings saved successfully!', 'success');
                loadSettings();
            } catch (error) {
                console.error('Error saving settings:', error);
                showNotification('Failed to save settings: ' + error.message, 'error');
            }
        }

        async function sendAdminMessage() {
            if (!currentUser || currentUserRole !== 'admin') {
                showNotification('You must be logged in as admin to send messages', 'error');
                return;
            }
            
            const messageText = elements.adminMessageInput.value.trim();
            if (!messageText) return;
            
            const conversationId = document.getElementById('admin-current-conversation').value;
            if (!conversationId) {
                showNotification('Please select a conversation first', 'error');
                return;
            }
            
            const messageData = {
                id: 'msg-' + Date.now(),
                conversationId: conversationId,
                senderId: currentUser.uid,
                senderName: 'Admin',
                senderRole: 'admin',
                content: messageText,
                timestamp: new Date().toISOString(),
                read: false
            };
            
            try {
                await saveMessageToFirebase(messageData);
                elements.adminMessageInput.value = '';
                loadAdminMessages(conversationId);
                showNotification('Message sent!', 'success');
            } catch (error) {
                console.error('Error sending message:', error);
                showNotification('Failed to send message: ' + error.message, 'error');
            }
        }

        async function sendCustomerMessage() {
            if (!currentUser || currentUserRole !== 'customer') {
                showNotification('You must be logged in as customer to send messages', 'error');
                return;
            }
            
            const messageText = elements.customerMessageInput.value.trim();
            if (!messageText) return;
            
            // For customers, they always message the admin
            // Create or get conversation with admin
            const conversationId = 'conversation-' + currentUser.uid;
            
            const messageData = {
                id: 'msg-' + Date.now(),
                conversationId: conversationId,
                senderId: currentUser.uid,
                senderName: currentUser.email,
                senderRole: 'customer',
                content: messageText,
                timestamp: new Date().toISOString(),
                read: false
            };
            
            try {
                await saveMessageToFirebase(messageData);
                elements.customerMessageInput.value = '';
                loadCustomerMessages();
                showNotification('Message sent to admin!', 'success');
            } catch (error) {
                console.error('Error sending message:', error);
                showNotification('Failed to send message: ' + error.message, 'error');
            }
        }

        async function saveArticleToFirebase(articleData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const articleRef = window.firebaseDBRef(window.firebaseDB, `articles/${articleData.id}`);
            await window.firebaseDBSet(articleRef, articleData);
        }

        async function updateArticleInFirebase(articleId, articleData) {
            if (!window.firebaseDBUpdate || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const articleRef = window.firebaseDBRef(window.firebaseDB, `articles/${articleId}`);
            await window.firebaseDBUpdate(articleRef, articleData);
        }

        async function saveBlogToFirebase(blogData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const blogRef = window.firebaseDBRef(window.firebaseDB, `blogs/${blogData.id}`);
            await window.firebaseDBSet(blogRef, blogData);
        }

        async function updateBlogInFirebase(blogId, blogData) {
            if (!window.firebaseDBUpdate || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const blogRef = window.firebaseDBRef(window.firebaseDB, `blogs/${blogId}`);
            await window.firebaseDBUpdate(blogRef, blogData);
        }

        async function saveLiteratureToFirebase(literatureData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const literatureRef = window.firebaseDBRef(window.firebaseDB, `literature/${literatureData.id}`);
            await window.firebaseDBSet(literatureRef, literatureData);
        }

        async function updateLiteratureInFirebase(literatureId, literatureData) {
            if (!window.firebaseDBUpdate || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const literatureRef = window.firebaseDBRef(window.firebaseDB, `literature/${literatureId}`);
            await window.firebaseDBUpdate(literatureRef, literatureData);
        }

        async function saveContentWritingProjectToFirebase(projectData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const projectRef = window.firebaseDBRef(window.firebaseDB, `contentWritingProjects/${projectData.id}`);
            await window.firebaseDBSet(projectRef, projectData);
        }

        async function saveSettingsToFirebase(settingsData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const settingsRef = window.firebaseDBRef(window.firebaseDB, 'settings');
            await window.firebaseDBSet(settingsRef, settingsData);
        }

        async function saveMediaToFirebase(mediaItem) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const mediaRef = window.firebaseDBRef(window.firebaseDB, `media/${mediaItem.id}`);
            await window.firebaseDBSet(mediaRef, mediaItem);
        }

        async function saveMessageToFirebase(messageData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const messageRef = window.firebaseDBRef(window.firebaseDB, `messages/${messageData.id}`);
            await window.firebaseDBSet(messageRef, messageData);
        }

        async function deleteMediaFromFirebase(mediaId, storagePath) {
            if (!window.firebaseDBRemove || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            // Delete from Database
            const mediaRef = window.firebaseDBRef(window.firebaseDB, `media/${mediaId}`);
            await window.firebaseDBRemove(mediaRef);
            
            // Delete from Storage if path exists
            if (storagePath && window.firebaseDeleteObject && window.firebaseStorageRef && window.firebaseStorage) {
                try {
                    const storageRef = window.firebaseStorageRef(window.firebaseStorage, storagePath);
                    await window.firebaseDeleteObject(storageRef);
                } catch (error) {
                    console.warn('Could not delete from storage:', error);
                }
            }
        }

        async function deleteArticleFromFirebase(articleId) {
            if (!window.firebaseDBRemove || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const articleRef = window.firebaseDBRef(window.firebaseDB, `articles/${articleId}`);
            await window.firebaseDBRemove(articleRef);
        }

        async function deleteBlogFromFirebase(blogId) {
            if (!window.firebaseDBRemove || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const blogRef = window.firebaseDBRef(window.firebaseDB, `blogs/${blogId}`);
            await window.firebaseDBRemove(blogRef);
        }

        async function deleteLiteratureFromFirebase(literatureId) {
            if (!window.firebaseDBRemove || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const literatureRef = window.firebaseDBRef(window.firebaseDB, `literature/${literatureId}`);
            await window.firebaseDBRemove(literatureRef);
        }

        async function deleteCustomerFromFirebase(customerId) {
            if (!window.firebaseDBRemove || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const customerRef = window.firebaseDBRef(window.firebaseDB, `customers/${customerId}`);
            await window.firebaseDBRemove(customerRef);
        }

        async function loadFromFirebase() {
            await loadArticlesFromFirebase();
            await loadBlogsFromFirebase();
            await loadLiteratureFromFirebase();
            await loadCustomersFromFirebase();
            await loadMediaFromFirebase();
            await loadContentWritingProjectsFromFirebase();
            await loadSettingsFromFirebase();
            await loadMessagesFromFirebase();
            updateDisplay();
        }

        async function loadArticlesFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading articles');
                return;
            }
            
            const articlesRef = window.firebaseDBRef(window.firebaseDB, 'articles');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(articlesRef, (snapshot) => {
                    const data = snapshot.val();
                    articles = data ? Object.values(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadBlogsFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading blogs');
                return;
            }
            
            const blogsRef = window.firebaseDBRef(window.firebaseDB, 'blogs');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(blogsRef, (snapshot) => {
                    const data = snapshot.val();
                    blogs = data ? Object.values(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadLiteratureFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading literature');
                return;
            }
            
            const literatureRef = window.firebaseDBRef(window.firebaseDB, 'literature');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(literatureRef, (snapshot) => {
                    const data = snapshot.val();
                    literature = data ? Object.values(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadCustomersFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading customers');
                return;
            }
            
            const customersRef = window.firebaseDBRef(window.firebaseDB, 'customers');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(customersRef, (snapshot) => {
                    const data = snapshot.val();
                    customers = data ? Object.entries(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadMediaFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading media');
                return;
            }
            
            const mediaRef = window.firebaseDBRef(window.firebaseDB, 'media');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(mediaRef, (snapshot) => {
                    const data = snapshot.val();
                    mediaFiles = data ? Object.values(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadContentWritingProjectsFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading content writing projects');
                return;
            }
            
            const projectsRef = window.firebaseDBRef(window.firebaseDB, 'contentWritingProjects');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(projectsRef, (snapshot) => {
                    const data = snapshot.val();
                    contentWritingProjects = data ? Object.values(data) : [];
                    updateDisplay();
                    resolve();
                });
            });
        }

        async function loadSettingsFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading settings');
                return;
            }
            
            const settingsRef = window.firebaseDBRef(window.firebaseDB, 'settings');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(settingsRef, (snapshot) => {
                    const data = snapshot.val();
                    settings = data || {};
                    updateSettingsForm();
                    resolve();
                });
            });
        }

        async function loadMessagesFromFirebase() {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                console.warn('Firebase Database not available for loading messages');
                return;
            }
            
            const messagesRef = window.firebaseDBRef(window.firebaseDB, 'messages');
            
            return new Promise((resolve) => {
                window.firebaseDBOnValue(messagesRef, (snapshot) => {
                    const data = snapshot.val();
                    messages = data ? Object.values(data) : [];
                    
                    // Update conversations for admin
                    if (currentUserRole === 'admin') {
                        updateAdminConversations();
                    }
                    
                    resolve();
                });
            });
        }

        function loadPublicContent() {
            // Load articles, blogs, and literature for customer view
            updateCustomerArticlesDisplay();
            updateCustomerBlogsDisplay();
            updateCustomerLiteratureDisplay();
        }

        async function loadCustomerProfile(userId) {
            if (!window.firebaseDBGet || !window.firebaseDBRef || !window.firebaseDB) {
                return;
            }
            
            try {
                const customerRef = window.firebaseDBRef(window.firebaseDB, `customers/${userId}`);
                const customerSnapshot = await window.firebaseDBGet(customerRef);
                
                if (customerSnapshot.exists()) {
                    const customerData = customerSnapshot.val();
                    elements.customerUserName.textContent = `Welcome, ${customerData.name}`;
                    
                    // Update customer profile content
                    elements.customerProfileContent.innerHTML = `
                        <div class="settings-grid">
                            <div>
                                <h3 style="color: var(--accent); margin-bottom: 1rem;">Personal Information</h3>
                                <p><strong>Full Name:</strong> ${customerData.name}</p>
                                <p><strong>Father Name:</strong> ${customerData.fatherName}</p>
                                <p><strong>Phone:</strong> ${customerData.phone}</p>
                                <p><strong>Email:</strong> ${customerData.email}</p>
                            </div>
                            <div>
                                <h3 style="color: var(--accent); margin-bottom: 1rem;">Address & Description</h3>
                                <p><strong>Address:</strong> ${customerData.address}</p>
                                <p><strong>Description:</strong> ${customerData.description || 'Not provided'}</p>
                                <p><strong>Member Since:</strong> ${new Date(customerData.createdAt).toLocaleDateString()}</p>
                            </div>
                        </div>
                    `;
                }
            } catch (error) {
                console.error('Error loading customer profile:', error);
            }
        }

        function updateSettingsForm() {
            if (settings) {
                document.getElementById('store-title').value = settings.title || '';
                document.getElementById('store-address').value = settings.address || '';
                document.getElementById('store-email').value = settings.email || '';
                document.getElementById('store-phone').value = settings.phone || '';
                document.getElementById('owner-name').value = settings.owner || '';
                document.getElementById('store-description').value = settings.description || '';
                document.getElementById('youtube-channel').value = settings.youtube || '';
                document.getElementById('whatsapp-channel').value = settings.whatsapp || '';
                document.getElementById('facebook-page').value = settings.facebook || '';
            }
        }

        function loadSettings() {
            // Update contact info section with settings
            if (settings) {
                elements.contactTitle.textContent = settings.title || 'The Power of Thoughts and Imagination';
                elements.contactAddress.textContent = settings.address || 'Not specified';
                elements.contactEmail.textContent = settings.email || 'samaddaud009@gmail.com';
                elements.contactPhone.textContent = settings.phone || '+92 324 7736384';
                elements.contactOwner.textContent = settings.owner || 'Samad Daud';
                elements.contactDescription.textContent = settings.description || 'Writer and Content Creator';
                
                // Update social links
                if (settings.youtube) {
                    elements.youtubeLink.href = settings.youtube;
                }
                if (settings.whatsapp) {
                    elements.whatsappLink.href = settings.whatsapp;
                }
                if (settings.facebook) {
                    elements.facebookLink.href = settings.facebook;
                }
            }
        }

        function updateAdminConversations() {
            if (currentUserRole !== 'admin') return;
            
            // Group messages by conversation
            const conversationsMap = {};
            
            messages.forEach(message => {
                if (!conversationsMap[message.conversationId]) {
                    conversationsMap[message.conversationId] = {
                        id: message.conversationId,
                        customerId: message.senderRole === 'customer' ? message.senderId : null,
                        customerName: message.senderRole === 'customer' ? message.senderName : 'Unknown',
                        lastMessage: message.content,
                        lastMessageTime: message.timestamp,
                        unreadCount: message.senderRole === 'customer' && !message.read ? 1 : 0,
                        messages: []
                    };
                } else {
                    // Update last message if this one is newer
                    if (new Date(message.timestamp) > new Date(conversationsMap[message.conversationId].lastMessageTime)) {
                        conversationsMap[message.conversationId].lastMessage = message.content;
                        conversationsMap[message.conversationId].lastMessageTime = message.timestamp;
                    }
                    
                    // Count unread messages
                    if (message.senderRole === 'customer' && !message.read) {
                        conversationsMap[message.conversationId].unreadCount++;
                    }
                }
                
                conversationsMap[message.conversationId].messages.push(message);
            });
            
            conversations = Object.values(conversationsMap);
            
            // Sort by last message time (newest first)
            conversations.sort((a, b) => new Date(b.lastMessageTime) - new Date(a.lastMessageTime));
            
            // Update UI
            if (conversations.length > 0) {
                elements.adminConversationsList.innerHTML = conversations.map(conv => `
                    <div class="conversation-item ${currentConversationId === conv.id ? 'active' : ''}" 
                         onclick="selectConversation('${conv.id}')">
                        <div class="conversation-header">
                            <span class="conversation-name">${conv.customerName}</span>
                            ${conv.unreadCount > 0 ? `<span class="unread-badge">${conv.unreadCount}</span>` : ''}
                        </div>
                        <div class="conversation-preview">${conv.lastMessage}</div>
                        <div class="conversation-date">${new Date(conv.lastMessageTime).toLocaleDateString()}</div>
                    </div>
                `).join('');
            } else {
                elements.adminConversationsList.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-comments"></i>
                        <h3>No Conversations</h3>
                        <p>Start a conversation with a customer.</p>
                    </div>
                `;
            }
        }

        function loadAdminMessages(conversationId) {
            if (!conversationId) return;
            
            const conversation = conversations.find(c => c.id === conversationId);
            if (!conversation) return;
            
            // Sort messages by timestamp
            conversation.messages.sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));
            
            // Update header
            elements.adminMessagesHeader.innerHTML = `
                <h3 style="margin: 0; color: var(--accent);">Conversation with ${conversation.customerName}</h3>
            `;
            
            // Update messages
            elements.adminMessagesContainer.innerHTML = conversation.messages.map(msg => `
                <div class="message ${msg.senderRole === 'admin' ? 'sent' : 'received'}">
                    <div class="message-header">
                        <span>${msg.senderName}</span>
                        <span>${new Date(msg.timestamp).toLocaleString()}</span>
                    </div>
                    <div class="message-content">${msg.content}</div>
                </div>
            `).join('');
            
            // Show message form
            elements.adminMessageForm.style.display = 'flex';
            document.getElementById('admin-current-conversation').value = conversationId;
            
            // Scroll to bottom
            elements.adminMessagesContainer.scrollTop = elements.adminMessagesContainer.scrollHeight;
            
            // Mark messages as read
            markMessagesAsRead(conversationId);
        }

        function loadCustomerMessages() {
            if (currentUserRole !== 'customer') return;
            
            const customerConversationId = 'conversation-' + currentUser.uid;
            const customerMessages = messages.filter(msg => msg.conversationId === customerConversationId);
            
            // Sort messages by timestamp
            customerMessages.sort((a, b) => new Date(a.timestamp) - new Date(b.timestamp));
            
            // Update messages
            if (customerMessages.length > 0) {
                elements.customerMessagesContainer.innerHTML = customerMessages.map(msg => `
                    <div class="message ${msg.senderRole === 'customer' ? 'sent' : 'received'}">
                        <div class="message-header">
                            <span>${msg.senderName}</span>
                            <span>${new Date(msg.timestamp).toLocaleString()}</span>
                        </div>
                        <div class="message-content">${msg.content}</div>
                    </div>
                `).join('');
            } else {
                elements.customerMessagesContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-comments"></i>
                        <h3>No Messages Yet</h3>
                        <p>Start a conversation with the admin!</p>
                    </div>
                `;
            }
            
            // Scroll to bottom
            elements.customerMessagesContainer.scrollTop = elements.customerMessagesContainer.scrollHeight;
        }

        async function markMessagesAsRead(conversationId) {
            if (!window.firebaseDBUpdate || !window.firebaseDBRef || !window.firebaseDB) {
                return;
            }
            
            try {
                // Mark all customer messages in this conversation as read
                const customerMessages = messages.filter(msg => 
                    msg.conversationId === conversationId && 
                    msg.senderRole === 'customer' && 
                    !msg.read
                );
                
                for (const msg of customerMessages) {
                    const messageRef = window.firebaseDBRef(window.firebaseDB, `messages/${msg.id}`);
                    await window.firebaseDBUpdate(messageRef, { read: true });
                }
            } catch (error) {
                console.error('Error marking messages as read:', error);
            }
        }

        function selectConversation(conversationId) {
            currentConversationId = conversationId;
            loadAdminMessages(conversationId);
            updateAdminConversations(); // Refresh to update active state
        }

        function editArticle(articleId) {
            const article = articles.find(a => a.id === articleId);
            if (article) {
                editingArticleId = articleId;
                showAdminSection('add-article-section');
                document.getElementById('article-form-title').textContent = 'Edit Article';
                document.getElementById('article-id').value = article.id;
                document.getElementById('article-title').value = article.title;
                document.getElementById('article-category').value = article.category;
                document.getElementById('article-content').value = article.content;
                document.getElementById('article-tags').value = article.tags?.join(', ') || '';
                
                const preview = document.getElementById('featured-image-preview');
                preview.innerHTML = `
                    <div class="preview-item">
                        <img src="${article.featuredImage}" alt="Featured image for ${article.title}">
                        <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            }
        }

        function editBlog(blogId) {
            const blog = blogs.find(b => b.id === blogId);
            if (blog) {
                editingBlogId = blogId;
                showAdminSection('add-blog-section');
                document.getElementById('blog-form-title').textContent = 'Edit Blog Post';
                document.getElementById('blog-id').value = blog.id;
                document.getElementById('blog-title').value = blog.title;
                document.getElementById('blog-category').value = blog.category;
                document.getElementById('blog-content').value = blog.content;
                document.getElementById('blog-tags').value = blog.tags?.join(', ') || '';
                
                const preview = document.getElementById('blog-featured-image-preview');
                preview.innerHTML = `
                    <div class="preview-item">
                        <img src="${blog.featuredImage}" alt="Featured image for ${blog.title}">
                        <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            }
        }

        function editLiterature(literatureId) {
            const lit = literature.find(l => l.id === literatureId);
            if (lit) {
                editingLiteratureId = literatureId;
                showAdminSection('add-literature-section');
                document.getElementById('literature-form-title').textContent = 'Edit Literature';
                document.getElementById('literature-id').value = lit.id;
                document.getElementById('literature-title').value = lit.title;
                document.getElementById('literature-type').value = lit.type;
                document.getElementById('literature-author').value = lit.author;
                document.getElementById('literature-year').value = lit.year;
                document.getElementById('literature-content').value = lit.content;
                document.getElementById('literature-summary').value = lit.summary || '';
                
                const preview = document.getElementById('literature-cover-image-preview');
                preview.innerHTML = `
                    <div class="preview-item">
                        <img src="${lit.coverImage}" alt="Cover image for ${lit.title}">
                        <button type="button" class="preview-remove" onclick="this.parentElement.remove()">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            }
        }

        function viewArticle(articleId) {
            const article = articles.find(a => a.id === articleId);
            if (article) {
                // Increment view count
                article.views = (article.views || 0) + 1;
                updateArticleInFirebase(article.id, article);
                
                const modalContent = document.getElementById('article-modal-content');
                modalContent.innerHTML = `
                    <article>
                        <h1 style="margin-bottom: 1rem;">${article.title}</h1>
                        <div style="display: flex; gap: 1.5rem; align-items: center; margin-bottom: 2.5rem; flex-wrap: wrap;">
                            <span class="badge">${article.category}</span>
                            <span class="text-muted"><i class="fas fa-user"></i> ${article.author}</span>
                            <span class="text-muted"><i class="fas fa-calendar"></i> ${new Date(article.createdAt).toLocaleDateString()}</span>
                            <span class="text-muted"><i class="fas fa-eye"></i> ${article.views} views</span>
                            <span class="text-muted"><i class="fas fa-heart"></i> ${article.likes} likes</span>
                        </div>
                        ${article.featuredImage ? `
                            <img src="${article.featuredImage}" alt="Featured image for ${article.title}" 
                                 style="width: 100%; height: 400px; object-fit: cover; margin-bottom: 2.5rem; border-radius: var(--border-radius);">
                        ` : ''}
                        <div style="line-height: 1.8; font-size: 1.15rem;">
                            ${article.content.split('\n').map(p => `<p style="margin-bottom: 1.5rem;">${p}</p>`).join('')}
                        </div>
                        ${article.tags && article.tags.length > 0 ? `
                            <div style="margin-top: 3rem; padding-top: 2rem; border-top: 1px solid rgba(0,0,0,0.1);">
                                <strong style="display: block; margin-bottom: 1rem;">Tags:</strong>
                                <div style="display: flex; gap: 0.75rem; flex-wrap: wrap;">
                                    ${article.tags.map(tag => `<span class="badge badge-secondary">${tag}</span>`).join('')}
                                </div>
                            </div>
                        ` : ''}
                        <div style="margin-top: 3rem; display: flex; gap: 1rem;">
                            <button class="btn btn-accent" onclick="likeContent('articles', '${article.id}')">
                                <i class="fas fa-heart"></i> Like (${article.likes || 0})
                            </button>
                            <button class="btn btn-success share-btn" onclick="shareContent('articles', '${article.id}', '${article.title}')">
                                <i class="fas fa-share"></i> Share Article
                            </button>
                        </div>
                        ${currentUserRole === 'customer' ? `
                            <div class="comments-section">
                                <h3 style="margin-bottom: 1.5rem;">Comments</h3>
                                <form class="comment-form" onsubmit="submitComment('articles', '${article.id}'); return false;">
                                    <div class="form-group">
                                        <textarea class="form-control" id="article-comment-${article.id}" placeholder="Add your comment..." rows="3" required></textarea>
                                    </div>
                                    <button type="submit" class="btn btn-success">
                                        <i class="fas fa-paper-plane"></i> Submit Comment
                                    </button>
                                </form>
                                <div class="comment-list" id="article-comments-${article.id}">
                                    <!-- Comments will be loaded here -->
                                </div>
                            </div>
                        ` : ''}
                    </article>
                `;
                document.getElementById('article-modal').classList.add('active');
                
                // Load comments if customer is logged in
                if (currentUserRole === 'customer') {
                    loadComments('articles', article.id);
                }
            }
        }

        function viewBlog(blogId) {
            const blog = blogs.find(b => b.id === blogId);
            if (blog) {
                // Increment view count
                blog.views = (blog.views || 0) + 1;
                updateBlogInFirebase(blog.id, blog);
                
                const modalContent = document.getElementById('blog-modal-content');
                modalContent.innerHTML = `
                    <article>
                        <h1 style="margin-bottom: 1rem;">${blog.title}</h1>
                        <div style="display: flex; gap: 1.5rem; align-items: center; margin-bottom: 2.5rem; flex-wrap: wrap;">
                            <span class="badge">${blog.category}</span>
                            <span class="text-muted"><i class="fas fa-user"></i> ${blog.author}</span>
                            <span class="text-muted"><i class="fas fa-calendar"></i> ${new Date(blog.createdAt).toLocaleDateString()}</span>
                            <span class="text-muted"><i class="fas fa-eye"></i> ${blog.views} views</span>
                            <span class="text-muted"><i class="fas fa-heart"></i> ${blog.likes} likes</span>
                        </div>
                        ${blog.featuredImage ? `
                            <img src="${blog.featuredImage}" alt="Featured image for ${blog.title}" 
                                 style="width: 100%; height: 400px; object-fit: cover; margin-bottom: 2.5rem; border-radius: var(--border-radius);">
                        ` : ''}
                        <div style="line-height: 1.8; font-size: 1.15rem;">
                            ${blog.content.split('\n').map(p => `<p style="margin-bottom: 1.5rem;">${p}</p>`).join('')}
                        </div>
                        ${blog.tags && blog.tags.length > 0 ? `
                            <div style="margin-top: 3rem; padding-top: 2rem; border-top: 1px solid rgba(0,0,0,0.1);">
                                <strong style="display: block; margin-bottom: 1rem;">Tags:</strong>
                                <div style="display: flex; gap: 0.75rem; flex-wrap: wrap;">
                                    ${blog.tags.map(tag => `<span class="badge badge-secondary">${tag}</span>`).join('')}
                                </div>
                            </div>
                        ` : ''}
                        <div style="margin-top: 3rem; display: flex; gap: 1rem;">
                            <button class="btn btn-accent" onclick="likeContent('blogs', '${blog.id}')">
                                <i class="fas fa-heart"></i> Like (${blog.likes || 0})
                            </button>
                            <button class="btn btn-success share-btn" onclick="shareContent('blogs', '${blog.id}', '${blog.title}')">
                                <i class="fas fa-share"></i> Share Blog
                            </button>
                        </div>
                        ${currentUserRole === 'customer' ? `
                            <div class="comments-section">
                                <h3 style="margin-bottom: 1.5rem;">Comments</h3>
                                <form class="comment-form" onsubmit="submitComment('blogs', '${blog.id}'); return false;">
                                    <div class="form-group">
                                        <textarea class="form-control" id="blog-comment-${blog.id}" placeholder="Add your comment..." rows="3" required></textarea>
                                    </div>
                                    <button type="submit" class="btn btn-success">
                                        <i class="fas fa-paper-plane"></i> Submit Comment
                                    </button>
                                </form>
                                <div class="comment-list" id="blog-comments-${blog.id}">
                                    <!-- Comments will be loaded here -->
                                </div>
                            </div>
                        ` : ''}
                    </article>
                `;
                document.getElementById('blog-modal').classList.add('active');
                
                // Load comments if customer is logged in
                if (currentUserRole === 'customer') {
                    loadComments('blogs', blog.id);
                }
            }
        }

        function viewLiterature(literatureId) {
            const lit = literature.find(l => l.id === literatureId);
            if (lit) {
                // Increment view count
                lit.views = (lit.views || 0) + 1;
                updateLiteratureInFirebase(lit.id, lit);
                
                const modalContent = document.getElementById('literature-modal-content');
                modalContent.innerHTML = `
                    <article>
                        <h1 style="margin-bottom: 1rem;">${lit.title}</h1>
                        <div style="display: flex; gap: 1.5rem; align-items: center; margin-bottom: 2.5rem; flex-wrap: wrap;">
                            <span class="badge">${lit.type}</span>
                            <span class="text-muted"><i class="fas fa-user"></i> ${lit.author}</span>
                            ${lit.year ? `<span class="text-muted"><i class="fas fa-calendar"></i> ${lit.year}</span>` : ''}
                            <span class="text-muted"><i class="fas fa-eye"></i> ${lit.views} views</span>
                            <span class="text-muted"><i class="fas fa-heart"></i> ${lit.likes} likes</span>
                        </div>
                        ${lit.coverImage ? `
                            <img src="${lit.coverImage}" alt="Cover image for ${lit.title}" 
                                 style="width: 100%; height: 400px; object-fit: cover; margin-bottom: 2.5rem; border-radius: var(--border-radius);">
                        ` : ''}
                        ${lit.summary ? `
                            <div style="background: rgba(233, 69, 96, 0.05); padding: 2rem; border-radius: var(--border-radius); margin-bottom: 2.5rem;">
                                <h3 style="color: var(--accent); margin-bottom: 1rem;">Summary</h3>
                                <p style="font-style: italic;">${lit.summary}</p>
                            </div>
                        ` : ''}
                        <div style="line-height: 1.8; font-size: 1.15rem;">
                            ${lit.content.split('\n').map(p => `<p style="margin-bottom: 1.5rem;">${p}</p>`).join('')}
                        </div>
                        <div style="margin-top: 3rem; display: flex; gap: 1rem;">
                            <button class="btn btn-accent" onclick="likeContent('literature', '${lit.id}')">
                                <i class="fas fa-heart"></i> Like (${lit.likes || 0})
                            </button>
                            <button class="btn btn-success share-btn" onclick="shareContent('literature', '${lit.id}', '${lit.title}')">
                                <i class="fas fa-share"></i> Share Literature
                            </button>
                        </div>
                        ${currentUserRole === 'customer' ? `
                            <div class="comments-section">
                                <h3 style="margin-bottom: 1.5rem;">Comments</h3>
                                <form class="comment-form" onsubmit="submitComment('literature', '${lit.id}'); return false;">
                                    <div class="form-group">
                                        <textarea class="form-control" id="literature-comment-${lit.id}" placeholder="Add your comment..." rows="3" required></textarea>
                                    </div>
                                    <button type="submit" class="btn btn-success">
                                        <i class="fas fa-paper-plane"></i> Submit Comment
                                    </button>
                                </form>
                                <div class="comment-list" id="literature-comments-${lit.id}">
                                    <!-- Comments will be loaded here -->
                                </div>
                            </div>
                        ` : ''}
                    </article>
                `;
                document.getElementById('literature-modal').classList.add('active');
                
                // Load comments if customer is logged in
                if (currentUserRole === 'customer') {
                    loadComments('literature', lit.id);
                }
            }
        }

        async function likeContent(type, id) {
            let content;
            let updateFunction;
            
            if (type === 'articles') {
                content = articles.find(a => a.id === id);
                updateFunction = updateArticleInFirebase;
            } else if (type === 'blogs') {
                content = blogs.find(b => b.id === id);
                updateFunction = updateBlogInFirebase;
            } else if (type === 'literature') {
                content = literature.find(l => l.id === id);
                updateFunction = updateLiteratureInFirebase;
            }
            
            if (content) {
                content.likes = (content.likes || 0) + 1;
                await updateFunction(content.id, content);
                
                // Update the modal if it's open
                if (type === 'articles' && document.getElementById('article-modal').classList.contains('active')) {
                    viewArticle(id);
                } else if (type === 'blogs' && document.getElementById('blog-modal').classList.contains('active')) {
                    viewBlog(id);
                } else if (type === 'literature' && document.getElementById('literature-modal').classList.contains('active')) {
                    viewLiterature(id);
                }
                
                showNotification('Thank you for your like!', 'success');
            }
        }

        async function submitComment(type, contentId) {
            if (!currentUser || currentUserRole !== 'customer') {
                showNotification('You must be logged in as a customer to comment', 'error');
                return;
            }
            
            const commentText = document.getElementById(`${type}-comment-${contentId}`).value;
            if (!commentText.trim()) {
                showNotification('Please enter a comment', 'error');
                return;
            }
            
            const commentData = {
                id: 'comment-' + Date.now(),
                content: commentText,
                author: currentUser.email,
                authorId: currentUser.uid,
                contentType: type,
                contentId: contentId,
                createdAt: new Date().toISOString()
            };
            
            try {
                await saveCommentToFirebase(commentData);
                document.getElementById(`${type}-comment-${contentId}`).value = '';
                showNotification('Comment submitted successfully!', 'success');
                loadComments(type, contentId);
            } catch (error) {
                console.error('Error submitting comment:', error);
                showNotification('Failed to submit comment: ' + error.message, 'error');
            }
        }

        async function saveCommentToFirebase(commentData) {
            if (!window.firebaseDBSet || !window.firebaseDBRef || !window.firebaseDB) {
                throw new Error('Firebase Database not available');
            }
            
            const commentRef = window.firebaseDBRef(window.firebaseDB, `comments/${commentData.id}`);
            await window.firebaseDBSet(commentRef, commentData);
        }

        async function loadComments(type, contentId) {
            if (!window.firebaseDBOnValue || !window.firebaseDBRef || !window.firebaseDB) {
                return;
            }
            
            const commentsRef = window.firebaseDBRef(window.firebaseDB, 'comments');
            
            window.firebaseDBOnValue(commentsRef, (snapshot) => {
                const data = snapshot.val();
                const allComments = data ? Object.values(data) : [];
                const contentComments = allComments.filter(comment => 
                    comment.contentType === type && comment.contentId === contentId
                ).sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
                
                const commentsContainer = document.getElementById(`${type}-comments-${contentId}`);
                if (commentsContainer) {
                    if (contentComments.length > 0) {
                        commentsContainer.innerHTML = contentComments.map(comment => `
                            <div class="comment-item">
                                <div class="comment-header">
                                    <span class="comment-author">${comment.author}</span>
                                    <span class="comment-date">${new Date(comment.createdAt).toLocaleDateString()}</span>
                                </div>
                                <div class="comment-content">${comment.content}</div>
                            </div>
                        `).join('');
                    } else {
                        commentsContainer.innerHTML = '<p class="text-muted">No comments yet. Be the first to comment!</p>';
                    }
                }
            });
        }

        async function deleteArticle(articleId) {
            if (confirm('Are you sure you want to delete this article? This action cannot be undone.')) {
                try {
                    await deleteArticleFromFirebase(articleId);
                    showNotification('Article deleted successfully', 'success');
                    await loadArticlesFromFirebase();
                } catch (error) {
                    console.error('Error deleting article:', error);
                    showNotification('Failed to delete article: ' + error.message, 'error');
                }
            }
        }

        async function deleteBlog(blogId) {
            if (confirm('Are you sure you want to delete this blog post? This action cannot be undone.')) {
                try {
                    await deleteBlogFromFirebase(blogId);
                    showNotification('Blog post deleted successfully', 'success');
                    await loadBlogsFromFirebase();
                } catch (error) {
                    console.error('Error deleting blog:', error);
                    showNotification('Failed to delete blog: ' + error.message, 'error');
                }
            }
        }

        async function deleteLiterature(literatureId) {
            if (confirm('Are you sure you want to delete this literature piece? This action cannot be undone.')) {
                try {
                    await deleteLiteratureFromFirebase(literatureId);
                    showNotification('Literature deleted successfully', 'success');
                    await loadLiteratureFromFirebase();
                } catch (error) {
                    console.error('Error deleting literature:', error);
                    showNotification('Failed to delete literature: ' + error.message, 'error');
                }
            }
        }

        async function deleteCustomer(customerId) {
            if (confirm('Are you sure you want to delete this customer? This action cannot be undone.')) {
                try {
                    await deleteCustomerFromFirebase(customerId);
                    showNotification('Customer deleted successfully', 'success');
                    await loadCustomersFromFirebase();
                } catch (error) {
                    console.error('Error deleting customer:', error);
                    showNotification('Failed to delete customer: ' + error.message, 'error');
                }
            }
        }

        async function deleteMedia(mediaId) {
            if (confirm('Are you sure you want to delete this media file?')) {
                try {
                    const mediaItem = mediaFiles.find(m => m.id === mediaId);
                    await deleteMediaFromFirebase(mediaId, mediaItem?.storagePath);
                    showNotification('Media file deleted', 'success');
                    await loadMediaFromFirebase();
                } catch (error) {
                    console.error('Error deleting media:', error);
                    showNotification('Failed to delete media: ' + error.message, 'error');
                }
            }
        }

        function shareContent(type, contentId, contentTitle) {
            const shareUrl = `${window.location.origin}${window.location.pathname}?${type}=${contentId}`;
            
            if (navigator.share) {
                // Use Web Share API if available
                navigator.share({
                    title: contentTitle,
                    text: 'Check out this content on MindScape',
                    url: shareUrl
                }).then(() => {
                    showNotification('Content shared successfully!', 'success');
                }).catch((error) => {
                    console.log('Sharing cancelled or failed:', error);
                });
            } else {
                // Fallback to clipboard
                navigator.clipboard.writeText(shareUrl).then(() => {
                    showNotification('Link copied to clipboard!', 'success');
                }).catch(() => {
                    // Final fallback
                    prompt('Copy this link to share:', shareUrl);
                });
            }
        }

        function updateDisplay() {
            updateArticlesDisplay(articles);
            updateBlogDisplay(blogs);
            updateLiteratureDisplay(literature);
            updateCustomersDisplay();
            updateContentWritingDisplay();
            updateMediaGallery();
            updatePublicMediaGallery();
            updateVideoDisplay();
            updateStats();
        }

        function updateArticlesDisplay(articlesToDisplay = articles) {
            const featuredContainer = elements.featuredArticlesContainer;
            const tableBody = elements.articlesTableBody;

            const publishedArticles = articlesToDisplay.filter(a => a.status === 'published');
            const featuredArticles = publishedArticles.slice(0, 3);

            // Update featured articles
            if (featuredArticles.length > 0) {
                featuredContainer.innerHTML = featuredArticles.map(article => `
                    <div class="featured-article card" onclick="viewArticle('${article.id}')">
                        <img src="${article.featuredImage}" alt="Featured image for ${article.title}">
                        <div class="featured-content">
                            <span class="badge">${article.category}</span>
                            <h3>${article.title}</h3>
                            <p>${article.content.substring(0, 120)}...</p>
                            <div style="display: flex; justify-content: space-between; align-items: center; margin-top: 1rem;">
                                <span><i class="fas fa-eye"></i> ${article.views} views</span>
                                <span>${new Date(article.createdAt).toLocaleDateString()}</span>
                            </div>
                        </div>
                    </div>
                `).join('');
            } else {
                featuredContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-newspaper"></i>
                        <h3>No Featured Articles Yet</h3>
                        <p>Create some amazing articles to feature here!</p>
                        <button class="btn btn-accent mt-3" onclick="showAddArticleForm()">
                            <i class="fas fa-pen"></i>Start Writing
                        </button>
                    </div>
                `;
            }

            // Update admin table
            if (articles.length > 0) {
                tableBody.innerHTML = articles.map(article => `
                    <tr>
                        <td><strong>${article.title}</strong></td>
                        <td><span class="badge">${article.category}</span></td>
                        <td>${new Date(article.createdAt).toLocaleDateString()}</td>
                        <td>${article.views || 0}</td>
                        <td><span class="badge badge-success">${article.status}</span></td>
                        <td>
                            <div class="table-actions">
                                <button class="btn btn-outline btn-sm" onclick="editArticle('${article.id}')" title="Edit Article">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-outline btn-sm" onclick="viewArticle('${article.id}')" title="View Article">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-danger btn-sm" onclick="deleteArticle('${article.id}')" title="Delete Article">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                            <i class="fas fa-newspaper" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                            <h4>No Articles Found</h4>
                            <p>Create your first article to get started!</p>
                            <button class="btn btn-success mt-2" onclick="showAddArticleForm()">
                                <i class="fas fa-plus"></i>Create First Article
                            </button>
                        </td>
                    </tr>
                `;
            }
        }

        function updateCustomerArticlesDisplay() {
            const container = elements.customerArticlesContainer;
            const publishedArticles = articles.filter(a => a.status === 'published');
            
            if (publishedArticles.length > 0) {
                container.innerHTML = publishedArticles.map(article => `
                    <div class="article-card card" onclick="viewArticle('${article.id}')">
                        <div class="article-img">
                            <img src="${article.featuredImage}" alt="Featured image for ${article.title}">
                        </div>
                        <div class="article-content">
                            <span class="badge" style="margin-bottom: 1rem;">${article.category}</span>
                            <h3>${article.title}</h3>
                            <p>${article.content.substring(0, 150)}...</p>
                            <div class="article-meta">
                                <span><i class="fas fa-calendar"></i> ${new Date(article.createdAt).toLocaleDateString()}</span>
                                <span><i class="fas fa-eye"></i> ${article.views || 0} views</span>
                            </div>
                        </div>
                    </div>
                `).join('');
            } else {
                container.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-newspaper"></i>
                        <h3>No Articles Available</h3>
                        <p>Check back later for new articles.</p>
                    </div>
                `;
            }
        }

        function updateBlogDisplay(blogsToDisplay = blogs) {
            const blogContainer = elements.blogContainer;
            const tableBody = elements.blogTableBody;

            const publishedBlogs = blogsToDisplay.filter(b => b.status === 'published');

            // Update blog display
            if (publishedBlogs.length > 0) {
                blogContainer.innerHTML = publishedBlogs.map(blog => `
                    <div class="blog-card" onclick="viewBlog('${blog.id}')">
                        <div class="article-img">
                            <img src="${blog.featuredImage}" alt="Featured image for ${blog.title}">
                        </div>
                        <div class="blog-content">
                            <span class="badge" style="margin-bottom: 1rem;">${blog.category}</span>
                            <h3>${blog.title}</h3>
                            <p>${blog.content.substring(0, 150)}...</p>
                            <div class="blog-meta">
                                <span><i class="fas fa-calendar"></i> ${new Date(blog.createdAt).toLocaleDateString()}</span>
                                <span><i class="fas fa-eye"></i> ${blog.views || 0} views</span>
                            </div>
                        </div>
                    </div>
                `).join('');
            } else {
                blogContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-blog"></i>
                        <h3>No Blog Posts Yet</h3>
                        <p>Share your daily thoughts and experiences with our community.</p>
                        <button class="btn btn-accent mt-3" onclick="showAddBlogForm()">
                            <i class="fas fa-pen"></i>Write First Blog
                        </button>
                    </div>
                `;
            }

            // Update admin table
            if (blogs.length > 0) {
                tableBody.innerHTML = blogs.map(blog => `
                    <tr>
                        <td><strong>${blog.title}</strong></td>
                        <td><span class="badge">${blog.category}</span></td>
                        <td>${new Date(blog.createdAt).toLocaleDateString()}</td>
                        <td>${blog.views || 0}</td>
                        <td><span class="badge badge-success">${blog.status}</span></td>
                        <td>
                            <div class="table-actions">
                                <button class="btn btn-outline btn-sm" onclick="editBlog('${blog.id}')" title="Edit Blog">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-outline btn-sm" onclick="viewBlog('${blog.id}')" title="View Blog">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-danger btn-sm" onclick="deleteBlog('${blog.id}')" title="Delete Blog">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                            <i class="fas fa-blog" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                            <h4>No Blog Posts Found</h4>
                            <p>Create your first blog post to get started!</p>
                            <button class="btn btn-success mt-2" onclick="showAddBlogForm()">
                                <i class="fas fa-plus"></i>Create First Blog
                            </button>
                        </td>
                    </tr>
                `;
            }
        }

        function updateCustomerBlogsDisplay() {
            const container = elements.customerBlogsContainer;
            const publishedBlogs = blogs.filter(b => b.status === 'published');
            
            if (publishedBlogs.length > 0) {
                container.innerHTML = publishedBlogs.map(blog => `
                    <div class="blog-card" onclick="viewBlog('${blog.id}')">
                        <div class="article-img">
                            <img src="${blog.featuredImage}" alt="Featured image for ${blog.title}">
                        </div>
                        <div class="blog-content">
                            <span class="badge" style="margin-bottom: 1rem;">${blog.category}</span>
                            <h3>${blog.title}</h3>
                            <p>${blog.content.substring(0, 150)}...</p>
                            <div class="blog-meta">
                                <span><i class="fas fa-calendar"></i> ${new Date(blog.createdAt).toLocaleDateString()}</span>
                                <span><i class="fas fa-eye"></i> ${blog.views || 0} views</span>
                            </div>
                        </div>
                    </div>
                `).join('');
            } else {
                container.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-blog"></i>
                        <h3>No Blog Posts Available</h3>
                        <p>Check back later for new blog posts.</p>
                    </div>
                `;
            }
        }

        function updateLiteratureDisplay(literatureToDisplay = literature) {
            const literatureContainer = elements.literatureContainer;
            const tableBody = elements.literatureTableBody;

            const publishedLiterature = literatureToDisplay.filter(l => l.status === 'published');

            // Update literature display
            if (publishedLiterature.length > 0) {
                literatureContainer.innerHTML = publishedLiterature.map(lit => `
                    <article class="article-card card" onclick="viewLiterature('${lit.id}')">
                        <div class="article-img">
                            <img src="${lit.coverImage}" alt="Cover image for ${lit.title}">
                        </div>
                        <div class="article-content">
                            <span class="badge" style="margin-bottom: 1rem;">${lit.type}</span>
                            <h3>${lit.title}</h3>
                            <p><strong>By ${lit.author}</strong></p>
                            <p>${lit.summary ? lit.summary.substring(0, 150) + '...' : lit.content.substring(0, 150) + '...'}</p>
                            <div class="article-meta">
                                <span><i class="fas fa-calendar"></i> ${lit.year || new Date(lit.createdAt).getFullYear()}</span>
                                <span><i class="fas fa-eye"></i> ${lit.views || 0} views</span>
                            </div>
                        </div>
                    </article>
                `).join('');
            } else {
                literatureContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-book"></i>
                        <h3>No Literature Pieces Yet</h3>
                        <p>Contribute to our growing collection of literary works.</p>
                        <button class="btn btn-accent mt-3" onclick="showAddLiteratureForm()">
                            <i class="fas fa-pen"></i>Contribute Literature
                        </button>
                    </div>
                `;
            }

            // Update admin table
            if (literature.length > 0) {
                tableBody.innerHTML = literature.map(lit => `
                    <tr>
                        <td><strong>${lit.title}</strong></td>
                        <td><span class="badge">${lit.type}</span></td>
                        <td>${lit.author}</td>
                        <td>${new Date(lit.createdAt).toLocaleDateString()}</td>
                        <td><span class="badge badge-success">${lit.status}</span></td>
                        <td>
                            <div class="table-actions">
                                <button class="btn btn-outline btn-sm" onclick="editLiterature('${lit.id}')" title="Edit Literature">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-outline btn-sm" onclick="viewLiterature('${lit.id}')" title="View Literature">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-danger btn-sm" onclick="deleteLiterature('${lit.id}')" title="Delete Literature">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                            <i class="fas fa-book" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                            <h4>No Literature Found</h4>
                            <p>Add your first literature piece to get started!</p>
                            <button class="btn btn-success mt-2" onclick="showAddLiteratureForm()">
                                <i class="fas fa-plus"></i>Add First Literature
                            </button>
                        </td>
                    </tr>
                `;
            }
        }

        function updateCustomerLiteratureDisplay() {
            const container = elements.customerLiteratureContainer;
            const publishedLiterature = literature.filter(l => l.status === 'published');
            
            if (publishedLiterature.length > 0) {
                container.innerHTML = publishedLiterature.map(lit => `
                    <article class="article-card card" onclick="viewLiterature('${lit.id}')">
                        <div class="article-img">
                            <img src="${lit.coverImage}" alt="Cover image for ${lit.title}">
                        </div>
                        <div class="article-content">
                            <span class="badge" style="margin-bottom: 1rem;">${lit.type}</span>
                            <h3>${lit.title}</h3>
                            <p><strong>By ${lit.author}</strong></p>
                            <p>${lit.summary ? lit.summary.substring(0, 150) + '...' : lit.content.substring(0, 150) + '...'}</p>
                            <div class="article-meta">
                                <span><i class="fas fa-calendar"></i> ${lit.year || new Date(lit.createdAt).getFullYear()}</span>
                                <span><i class="fas fa-eye"></i> ${lit.views || 0} views</span>
                            </div>
                        </div>
                    </article>
                `).join('');
            } else {
                container.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-book"></i>
                        <h3>No Literature Available</h3>
                        <p>Check back later for new literature pieces.</p>
                    </div>
                `;
            }
        }

        function updateCustomersDisplay() {
            const tableBody = elements.customersTableBody;

            if (customers.length > 0) {
                tableBody.innerHTML = customers.map(([id, customer]) => `
                    <tr>
                        <td><strong>${customer.name}</strong></td>
                        <td>${customer.fatherName}</td>
                        <td>${customer.phone}</td>
                        <td>${customer.email}</td>
                        <td>${customer.address}</td>
                        <td>
                            <div class="table-actions">
                                <button class="btn btn-outline btn-sm" onclick="viewCustomer('${id}')" title="View Customer">
                                    <i class="fas fa-eye"></i>
                                </button>
                                <button class="btn btn-danger btn-sm" onclick="deleteCustomer('${id}')" title="Delete Customer">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                            <i class="fas fa-users" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                            <h4>No Customers Found</h4>
                            <p>No customers have registered yet.</p>
                        </td>
                    </tr>
                `;
            }
        }

        function updateContentWritingDisplay() {
            const tableBody = elements.contentWritingTableBody;

            if (contentWritingProjects.length > 0) {
                tableBody.innerHTML = contentWritingProjects.map(project => `
                    <tr>
                        <td><strong>${project.name}</strong></td>
                        <td>${project.client}</td>
                        <td><span class="badge">${project.type}</span></td>
                        <td>${new Date(project.deadline).toLocaleDateString()}</td>
                        <td><span class="badge badge-success">${project.status}</span></td>
                        <td>
                            <div class="table-actions">
                                <button class="btn btn-outline btn-sm" title="Edit Project">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button class="btn btn-danger btn-sm" title="Delete Project">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 3rem;">
                            <i class="fas fa-pen-fancy" style="font-size: 3rem; margin-bottom: 1rem; display: block;"></i>
                            <h4>No Content Writing Projects</h4>
                            <p>Start your first content writing project!</p>
                        </td>
                    </tr>
                `;
            }
        }

        function updateMediaGallery() {
            const gallery = elements.mediaGallery;
            if (mediaFiles.length > 0) {
                gallery.innerHTML = mediaFiles.map(media => {
                    if (media.type.startsWith('image/')) {
                        return `
                            <div class="preview-item">
                                <img src="${media.url}" alt="Media library image: ${media.name}">
                                <button class="preview-remove" onclick="deleteMedia('${media.id}')" title="Delete Media">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                        `;
                    } else {
                        return `
                            <div class="preview-item">
                                <video controls style="width: 100%; height: 100%; object-fit: cover;">
                                    <source src="${media.url}" type="${media.type}">
                                    Your browser does not support the video tag.
                                </video>
                                <button class="preview-remove" onclick="deleteMedia('${media.id}')" title="Delete Media">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                        `;
                    }
                }).join('');
            } else {
                gallery.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-images"></i>
                        <h3>No Media Files</h3>
                        <p>Upload images and videos to use in your content.</p>
                    </div>
                `;
            }
        }

        function updatePublicMediaGallery() {
            const gallery = elements.publicMediaGallery;
            const images = mediaFiles.filter(m => m.type.startsWith('image/'));
            
            if (images.length > 0) {
                gallery.innerHTML = images.map(media => `
                    <div class="preview-item">
                        <img src="${media.url}" alt="Gallery image: ${media.name}">
                    </div>
                `).join('');
            } else {
                gallery.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-images"></i>
                        <h3>No Images Yet</h3>
                        <p>Upload images to showcase in your articles and blog posts.</p>
                    </div>
                `;
            }
        }

        function updateVideoDisplay() {
            const videoContainer = elements.videoContainer;
            const videos = mediaFiles.filter(m => m.type.startsWith('video/'));
            
            if (videos.length > 0) {
                videoContainer.innerHTML = videos.map(media => `
                    <div class="video-card">
                        <div class="video-placeholder">
                            <i class="fas fa-play-circle"></i>
                        </div>
                        <div class="video-content">
                            <h4>${media.name}</h4>
                            <p>Uploaded: ${new Date(media.uploadedAt).toLocaleDateString()}</p>
                        </div>
                    </div>
                `).join('');
            } else {
                videoContainer.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-video"></i>
                        <h3>No Videos Yet</h3>
                        <p>Share video content to complement your written work.</p>
                    </div>
                `;
            }
        }

        function updateStats() {
            document.getElementById('total-articles').textContent = articles.length;
            document.getElementById('total-blogs').textContent = blogs.length;
            document.getElementById('total-customers').textContent = customers.length;
            document.getElementById('total-media').textContent = mediaFiles.length;
            
            // Calculate additional stats
            const monthlyViews = articles.reduce((sum, article) => {
                const articleDate = new Date(article.createdAt);
                const currentDate = new Date();
                if (articleDate.getMonth() === currentDate.getMonth() && 
                    articleDate.getFullYear() === currentDate.getFullYear()) {
                    return sum + (article.views || 0);
                }
                return sum;
            }, 0) + blogs.reduce((sum, blog) => {
                const blogDate = new Date(blog.createdAt);
                const currentDate = new Date();
                if (blogDate.getMonth() === currentDate.getMonth() && 
                    blogDate.getFullYear() === currentDate.getFullYear()) {
                    return sum + (blog.views || 0);
                }
                return sum;
            }, 0) + literature.reduce((sum, lit) => {
                const litDate = new Date(lit.createdAt);
                const currentDate = new Date();
                if (litDate.getMonth() === currentDate.getMonth() && 
                    litDate.getFullYear() === currentDate.getFullYear()) {
                    return sum + (lit.views || 0);
                }
                return sum;
            }, 0);
            
            const totalContentLength = articles.reduce((sum, article) => sum + (article.content?.length || 0), 0) +
                                     blogs.reduce((sum, blog) => sum + (blog.content?.length || 0), 0) +
                                     literature.reduce((sum, lit) => sum + (lit.content?.length || 0), 0);
            
            const totalContentItems = articles.length + blogs.length + literature.length;
            const avgReadTime = totalContentItems > 0 ? 
                Math.round(totalContentLength / (totalContentItems * 200)) : 0;
            
            const totalShares = articles.reduce((sum, article) => sum + (article.shares || 0), 0) +
                              blogs.reduce((sum, blog) => sum + (blog.shares || 0), 0) +
                              literature.reduce((sum, lit) => sum + (lit.shares || 0), 0);
            
            const totalLikes = articles.reduce((sum, article) => sum + (article.likes || 0), 0) +
                              blogs.reduce((sum, blog) => sum + (blog.likes || 0), 0) +
                              literature.reduce((sum, lit) => sum + (lit.likes || 0), 0);
            
            document.getElementById('monthly-views').textContent = monthlyViews;
            document.getElementById('avg-read-time').textContent = avgReadTime + 'm';
            document.getElementById('total-shares').textContent = totalShares;
            document.getElementById('total-likes').textContent = totalLikes;
            
            // Update analytics table
            const analyticsTableBody = document.getElementById('analytics-table-body');
            const allContent = [
                ...articles.map(a => ({...a, type: 'Article'})),
                ...blogs.map(b => ({...b, type: 'Blog'})),
                ...literature.map(l => ({...l, type: 'Literature'}))
            ].sort((a, b) => (b.views || 0) - (a.views || 0)).slice(0, 5);
            
            if (allContent.length > 0) {
                analyticsTableBody.innerHTML = allContent.map(item => `
                    <tr>
                        <td><strong>${item.title}</strong></td>
                        <td><span class="badge">${item.type}</span></td>
                        <td>${item.views || 0}</td>
                        <td>${item.likes || 0}</td>
                        <td>${item.shares || 0}</td>
                        <td>
                            <div style="display: flex; align-items: center; gap: 0.5rem;">
                                <div style="width: 100px; height: 8px; background: #e2e8f0; border-radius: 4px; overflow: hidden;">
                                    <div style="height: 100%; width: ${Math.min(100, (item.views || 0) / 10)}%; background: var(--accent);"></div>
                                </div>
                                <span>${Math.min(100, (item.views || 0) / 10)}%</span>
                            </div>
                        </td>
                    </tr>
                `).join('');
            } else {
                analyticsTableBody.innerHTML = `
                    <tr>
                        <td colspan="6" class="text-center text-muted" style="padding: 2rem;">
                            <i class="fas fa-chart-bar" style="font-size: 2.5rem; margin-bottom: 1rem; display: block;"></i>
                            <p>No content data available yet</p>
                        </td>
                    </tr>
                `;
            }
        }

        function showNotification(message, type = 'success') {
            // Remove existing notifications
            document.querySelectorAll('.notification').forEach(n => n.remove());
            
            const notification = document.createElement('div');
            notification.className = `notification ${type}`;
            notification.innerHTML = `
                <i class="fas fa-${type === 'success' ? 'check-circle' : type === 'error' ? 'exclamation-triangle' : 'info-circle'}"></i>
                <span>${message}</span>
            `;
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.style.animation = 'slideInRight 0.3s ease reverse';
                setTimeout(() => notification.remove(), 300);
            }, 4000);
        }

        // Customer view function
        function viewCustomer(customerId) {
            const customer = customers.find(([id]) => id === customerId);
            if (customer) {
                const data = customer[1];
                alert(`
                    Customer Details:
                    Name: ${data.name}
                    Father Name: ${data.fatherName}
                    Phone: ${data.phone}
                    Email: ${data.email}
                    Address: ${data.address}
                    Description: ${data.description || "N/A"}
                    Member Since: ${new Date(data.createdAt).toLocaleDateString()}
                `);
            }
        }

        // Make functions globally available for HTML onclick attributes
        window.editArticle = editArticle;
        window.editBlog = editBlog;
        window.editLiterature = editLiterature;
        window.viewArticle = viewArticle;
        window.viewBlog = viewBlog;
        window.viewLiterature = viewLiterature;
        window.deleteArticle = deleteArticle;
        window.deleteBlog = deleteBlog;
        window.deleteLiterature = deleteLiterature;
        window.deleteCustomer = deleteCustomer;
        window.deleteMedia = deleteMedia;
        window.closeModal = closeModal;
        window.showAddArticleForm = showAddArticleForm;
        window.showAddBlogForm = showAddBlogForm;
        window.showAddLiteratureForm = showAddLiteratureForm;
        window.showMediaSection = showMediaSection;
        window.likeContent = likeContent;
        window.shareContent = shareContent;
        window.submitComment = submitComment;
        window.selectConversation = selectConversation;
        window.viewCustomer = viewCustomer;
    </script>
</body>
</html>
