# Website-script.js
Website script.js
// your contacts tokens
const tokenAddress = "0xYourTokenAddressHere";

// Minimal ERC20 ABI
const tokenABI = [
    "function name() view returns (string)",
    "function symbol() view returns (string)",
    "function totalSupply() view returns (uint256)",
    "function balanceOf(address) view returns (uint256)"
];

let provider;
let signer;

window.onload = async () => {
    document.getElementById("tokenAddress").innerText = tokenAddress;

    document.getElementById("connectButton").onclick = async () => {
        if (window.ethereum) {
            provider = new ethers.providers.Web3Provider(window.ethereum);
            await provider.send("eth_requestAccounts", []);
            signer = provider.getSigner();
            updateTokenInfo();
        } else {
            alert("Please install MetaMask!");
        }
    }
};

async function updateTokenInfo() {
    const contract = new ethers.Contract(tokenAddress, tokenABI, signer);

    const name = await contract.name();
    const symbol = await contract.symbol();
    const totalSupply = await contract.totalSupply();
    const userAddress = await signer.getAddress();
    const userBalance = await contract.balanceOf(userAddress);

    document.getElementById("tokenName").innerText = name;
    document.getElementById("tokenSymbol").innerText = symbol;
    document.getElementById("totalSupply").innerText = ethers.utils.formatUnits(totalSupply, 18);
    document.getElementById("userBalance").innerText = ethers.utils.formatUnits(userBalance, 18);
}
