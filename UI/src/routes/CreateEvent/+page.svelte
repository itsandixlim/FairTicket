<!-- Obtained via ChatGPT and modified to fit needs -->
<script lang=ts>
    import { ethers } from "ethers";
    import type { JsonRpcSigner } from "ethers";
    import { Contract } from "ethers";
    import { ABI } from "../abi";

    let tokenURI = '';
    let totalTickets = '';
    let ticketPrice = '';
    let ticketEndDate = '';

    async function handleSubmit() {
        const eventData = {
            tokenURI: tokenURI,
            totalTickets: totalTickets,
            ticketPrice: ticketPrice,
            ticketEndDate: ticketEndDate,
        };
        
        console.log('Event Data:', eventData);

        const { ethereum } = window as any;
        const provider = new ethers.BrowserProvider(ethereum);
        const signer = await provider.getSigner();
        const contract = await initializeContract(signer);

        let totalTickets2 = parseInt(totalTickets);
        let ticketPrice2 = parseFloat(ticketPrice);

        console.log("To create");

        await contract.createTicket(tokenURI, totalTickets2, ticketPrice2, ticketEndDate, {value : 10});

        alert("Successfully created new event!")
    }

    const initializeContract = async (signer: JsonRpcSigner) => {
        return new Contract(
        "0xBc831cF371080D11f081b79cb52614F3FFD99D44",
        ABI,
        signer
        );
    };
</script>

<svelte:head>
    <title>Create Event</title>
    <meta name="CreateEvent" content="Form to create a new event" />
</svelte:head>

<div class="text-column">
    <h1>Create a New Event</h1>
    <form on:submit|preventDefault={handleSubmit}>
        <div>
            <label for="tokenURI">Event Name:</label>
            <input id="tokenURI" type="text" bind:value={tokenURI} required />
        </div>
        <div>
            <label for="totalTikets">Total Tickets:</label>
            <input id="totalTickets" type="number" step="1" min="0" bind:value={totalTickets} required />
        </div>
        <div>
            <!-- I think its in Wei -->
            <label for="ticketPrice">Ticket Price in Wei:</label>
            <input id="ticketPrice" type="number" step="1" min="0" bind:value={ticketPrice} required />
        </div>
        <div>
            <label for="ticketEndDate">Ticket Expiry Timestamp:</label>
            <input id="ticketEndDate" type="number" step="1" bind:value={ticketEndDate} required />
        </div>
        
        <div class="button-container">
            <button type="submit">Create Event</button>
        </div>
    </form>
</div>

<style>
    .text-column {
        max-width: 600px;
        margin: 0 auto;
        padding: 20px;
        font-family: Arial, sans-serif;
    }
    label {
        display: block;
        margin-bottom: 8px;
        font-weight: bold;
    }
    input, textarea {
        width: 100%;
        padding: 8px;
        margin-bottom: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
    .button-container {
        display: flex;
        justify-content: center;
    }
    button {
        padding: 10px 20px;
        background-color: #007BFF;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
    button:hover {
        background-color: #0056b3;
    }
</style>