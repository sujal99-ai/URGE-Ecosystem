<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UrgeTapTap</title>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-storage-compat.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
    <!-- Ad SDK -->
    <script src='//libtl.com/sdk.js' data-zone='10093989' data-sdk='show_10093989'></script>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --primary: #2563eb;
            --primary-dark: #1d4ed8;
            --primary-light: #3b82f6;
            --secondary: #7e22ce;
            --dark-bg: #0f172a;
            --dark-card: #1e293b;
            --dark-card-light: #334155;
            --text: #f8fafc;
            --text-secondary: #94a3b8;
            --success: #10b981;
            --warning: #f59e0b;
            --error: #ef4444;
            --silver: #cbd5e1;
            --gold: #fbbf24;
            --platinum: #e5e7eb;
            --diamond: #7dd3fc;
            --legendary: #f97316;
            --god: #a855f7;
        }

        body {
            background: linear-gradient(135deg, var(--dark-bg), #1e293b, #334155);
            color: var(--text);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            padding-bottom: 80px;
            overflow-x: hidden;
        }

        .bg-animation {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.3;
        }

        .bg-circle {
            position: absolute;
            border-radius: 50%;
            background: rgba(37, 99, 235, 0.1);
            animation: float 6s ease-in-out infinite;
        }

        .bg-circle:nth-child(1) {
            width: 60px;
            height: 60px;
            top: 20%;
            left: 10%;
            animation-delay: 0s;
        }

        .bg-circle:nth-child(2) {
            width: 80px;
            height: 80px;
            top: 60%;
            left: 80%;
            animation-delay: 2s;
        }

        .bg-circle:nth-child(3) {
            width: 100px;
            height: 100px;
            top: 40%;
            left: 60%;
            animation-delay: 4s;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-15px) scale(1.05); }
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 15px;
        }

        .hidden {
            display: none !important;
        }

        /* Header Styles */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 15px;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(15px);
            border-bottom: 1px solid rgba(37, 99, 235, 0.3);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo-section {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .logo {
            width: 40px;
            height: 40px;
            background: linear-gradient(145deg, var(--primary), var(--primary-dark));
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: var(--dark-bg);
            box-shadow: 0 0 12px rgba(37, 99, 235, 0.7);
            font-size: 18px;
        }

        .app-title {
            font-size: 18px;
            font-weight: 700;
            background: linear-gradient(to right, var(--primary), var(--primary-light));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 8px rgba(37, 99, 235, 0.3);
        }

        .points-display {
            display: flex;
            gap: 10px;
        }

        .points-item {
            display: flex;
            align-items: center;
            gap: 6px;
            background: rgba(255, 255, 255, 0.08);
            padding: 8px 12px;
            border-radius: 10px;
            font-size: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
        }

        /* Auth Pages Styles */
        .auth-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 15px;
        }

        .auth-card {
            background: rgba(30, 41, 59, 0.95);
            border: 1px solid rgba(37, 99, 235, 0.2);
            backdrop-filter: blur(15px);
            padding: 30px 25px;
            border-radius: 18px;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.5);
            position: relative;
            overflow: hidden;
        }

        .auth-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 4px;
            background: linear-gradient(to right, var(--primary), var(--primary-dark));
        }

        .auth-header {
            text-align: center;
            margin-bottom: 25px;
        }

        .auth-title { 
            text-align: center; 
            margin-bottom: 8px; 
            font-size: 26px;
            font-weight: 700;
            background: linear-gradient(to right, var(--primary), var(--primary-light));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .auth-subtitle {
            text-align: center;
            color: var(--text-secondary);
            font-size: 14px;
            margin-bottom: 20px;
        }

        .form-group { 
            margin-bottom: 20px; 
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-size: 14px;
            opacity: 0.9;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid rgba(255, 255, 255, 0.15);
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.08);
            color: white;
            font-size: 15px;
            transition: all 0.3s ease;
        }

        .form-control:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.2);
            background: rgba(255, 255, 255, 0.12);
        }

        .btn {
            padding: 14px 20px;
            border: none;
            border-radius: 10px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(145deg, var(--primary), var(--primary-dark));
            color: white;
            width: 100%;
            box-shadow: 0 5px 12px rgba(37, 99, 235, 0.4);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 7px 15px rgba(37, 99, 235, 0.6);
        }

        .auth-footer { 
            text-align: center; 
            margin-top: 20px; 
        }

        .auth-link { 
            color: var(--primary); 
            text-decoration: none; 
            font-weight: 600; 
        }

        .auth-link:hover { 
            text-decoration: underline; 
        }

        .referral-section {
            background: rgba(37, 99, 235, 0.1);
            border-radius: 12px;
            padding: 15px;
            margin-top: 20px;
            border: 1px solid rgba(37, 99, 235, 0.2);
        }

        .referral-title {
            text-align: center;
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 10px;
            color: var(--primary);
        }

        .referral-benefits {
            display: flex;
            justify-content: space-around;
            text-align: center;
            margin-bottom: 15px;
        }

        .benefit-item {
            flex: 1;
            padding: 0 10px;
        }

        .benefit-value {
            font-size: 18px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 5px;
        }

        .benefit-label {
            font-size: 12px;
            color: var(--text-secondary);
        }

        /* Main App Styles */
        .main-app {
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .section {
            display: none;
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            max-width: 100%;
        }

        .section.active {
            display: block;
        }

        .section-title {
            text-align: center;
            margin-bottom: 20px;
            font-size: 24px;
            font-weight: 700;
            color: var(--primary);
            text-shadow: 0 0 8px rgba(37, 99, 235, 0.3);
        }

        .tap-area {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 15px;
            text-align: center;
        }

        .tap-counter {
            font-size: 48px;
            font-weight: 800;
            margin-bottom: 12px;
            text-shadow: 0 0 12px rgba(37, 99, 235, 0.8);
            background: linear-gradient(to bottom, var(--primary), var(--primary-light));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .level-display {
            font-size: 18px;
            margin-bottom: 30px;
            background: rgba(0, 0, 0, 0.3);
            padding: 8px 20px;
            border-radius: 18px;
            border: 1px solid rgba(37, 99, 235, 0.3);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        .tap-button {
            width: 220px;
            height: 220px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 44px;
            font-weight: 800;
            color: white;
            text-shadow: 0px 2px 3px rgba(0, 0, 0, 0.5);
            cursor: pointer;
            box-shadow: 0 0 35px rgba(37, 99, 235, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
            border: 4px solid var(--primary-light);
            user-select: none;
            transition: all 0.1s ease;
            animation: pulse 2s infinite ease-in-out;
            position: relative;
            overflow: hidden;
        }

        .tap-button::after {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, rgba(255,255,255,0) 70%);
            transform: rotate(30deg);
            opacity: 0;
            transition: opacity 0.3s;
        }

        .tap-button:active::after {
            opacity: 1;
        }

        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 35px rgba(37, 99, 235, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4); }
            50% { transform: scale(1.04); box-shadow: 0 0 45px rgba(37, 99, 235, 1), 0 12px 30px rgba(0, 0, 0, 0.5); }
            100% { transform: scale(1); box-shadow: 0 0 35px rgba(37, 99, 235, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4); }
        }

        .tap-button:active {
            transform: scale(0.95);
            box-shadow: 0 0 20px rgba(37, 99, 235, 0.7), 0 4px 15px rgba(0, 0, 0, 0.3);
            animation: none;
        }

        /* Dynamic tap button colors based on tier */
        .tap-button.silver {
            background: linear-gradient(145deg, #f0f0f0, #C0C0C0, #a0a0a0);
            border-color: #f0f0f0;
            box-shadow: 0 0 35px rgba(192, 192, 192, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .tap-button.gold {
            background: linear-gradient(145deg, #fff9c4, #FFD700, #cc9900);
            border-color: #fff9c4;
            box-shadow: 0 0 35px rgba(255, 215, 0, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .tap-button.platinum {
            background: linear-gradient(145deg, #f5f5f5, #E5E4E2, #c0c0c0);
            border-color: #f5f5f5;
            box-shadow: 0 0 35px rgba(229, 228, 226, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .tap-button.diamond {
            background: linear-gradient(145deg, #e6f7ff, #b9f2ff, #7ec4cf);
            border-color: #e6f7ff;
            box-shadow: 0 0 35px rgba(185, 242, 255, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .tap-button.legendary {
            background: linear-gradient(145deg, #ffcccc, #ff6b6b, #cc3333);
            border-color: #ffcccc;
            box-shadow: 0 0 35px rgba(255, 107, 107, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .tap-button.god {
            background: linear-gradient(145deg, #e6ccff, #9d4edd, #7b2cbf);
            border-color: #e6ccff;
            box-shadow: 0 0 35px rgba(157, 78, 221, 0.9), 0 8px 25px rgba(0, 0, 0, 0.4);
        }

        .progress-bar {
            width: 90%;
            max-width: 380px;
            height: 14px;
            background: rgba(0, 0, 0, 0.4);
            border-radius: 8px;
            margin-top: 40px;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(to right, var(--primary), var(--primary-dark));
            box-shadow: 0 0 12px var(--primary);
            width: 30%;
            transition: width 0.4s ease;
            border-radius: 8px;
        }

        /* Boost Section Styles */
        .boost-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .boost-card {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .boost-card:hover {
            transform: translateY(-5px);
            border-color: var(--primary);
            box-shadow: 0 5px 15px rgba(37, 99, 235, 0.3);
        }

        .boost-icon {
            font-size: 32px;
            margin-bottom: 10px;
            color: var(--primary);
        }

        .boost-name {
            font-weight: 600;
            margin-bottom: 8px;
        }

        .boost-multiplier {
            font-size: 14px;
            color: var(--primary);
            margin-bottom: 8px;
        }

        .boost-duration {
            font-size: 12px;
            color: var(--text-secondary);
            margin-bottom: 12px;
        }

        .boost-cost {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            font-size: 14px;
            font-weight: 600;
        }

        .active-boosts {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
            padding: 15px;
            margin-top: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .active-boosts-title {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 10px;
            text-align: center;
        }

        .active-boost-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .active-boost-item:last-child {
            border-bottom: none;
        }

        /* Staking Section Styles */
        .staking-container {
            max-width: 100%;
            margin: 0 auto;
        }

        .staking-tabs {
            display: flex;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
            padding: 5px;
            margin-bottom: 20px;
        }

        .staking-tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
        }

        .staking-tab.active {
            background: var(--primary);
            color: white;
        }

        .staking-content {
            display: none;
        }

        .staking-content.active {
            display: block;
        }

        .staking-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 18px;
            padding: 20px;
            margin-bottom: 20px;
            border: 1px solid rgba(37, 99, 235, 0.2);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
        }

        .staking-stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .staking-stat {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
            padding: 15px;
            text-align: center;
        }

        .staking-stat-value {
            font-size: 20px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 5px;
        }

        .staking-stat-label {
            font-size: 12px;
            color: var(--text-secondary);
        }

        /* Multi-Step Deposit Process */
        .deposit-process {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .step-indicator {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            position: relative;
        }

        .step-indicator::before {
            content: '';
            position: absolute;
            top: 15px;
            left: 0;
            width: 100%;
            height: 2px;
            background: rgba(255, 255, 255, 0.2);
            z-index: 1;
        }

        .step {
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
            z-index: 2;
        }

        .step-number {
            width: 30px;
            height: 30px;
            border-radius: 50%;
 
