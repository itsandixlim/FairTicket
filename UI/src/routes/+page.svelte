<script>
    import { goto } from '$app/navigation';

    let userAddress = null;

    async function connectWallet() {
        if (window.ethereum) {
            const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
            if (accounts.length > 0) {
                userAddress = accounts[0];
            } else {
                alert('No accounts found');
            }
        } else {
            alert('No ethereum wallet found');
        }
    }
</script>

<style>
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        background: #f4f4f9;
        text-align: center;
        font-family: Arial, sans-serif;
    }

    h1 {
        font-size: 2.5rem;
        color: #333;
        margin-bottom: 1rem;
    }

    p {
        font-size: 1.25rem;
        color: #4caf50;
    }

    button {
        background-color: #1e90ff;
        color: #fff;
        padding: 10px 20px;
        font-size: 1rem;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        transition: background-color 0.3s ease;
    }

    button:hover {
        background-color: #005bbb;
    }

    .options {
        margin-top: 2rem;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .options button {
        background-color: #ff9800;
    }

    .options button:hover {
        background-color: #e68900;
    }
</style>

<div class="container">
    <h1>Welcome to FairTicketing!</h1>
    
    {#if userAddress}
        <p>
            Successfully connected with account <strong>{userAddress}</strong>
        </p>
        <div class="options">
            <button on:click={() => goto('/EventList')}>Purchase ticket from existing events</button>
            <button on:click={() => goto('/CreateEvent')}>Create new event</button>
            <button on:click={() => goto('/ResellTicket')}>Resell your ticket</button>
        </div>
    {:else}
        <button on:click={connectWallet}>
            Connect with Wallet
        </button>
    {/if}
</div>
