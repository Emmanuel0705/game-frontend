<script>
import Icon from 'svelte-icons-pack/Icon.svelte';
import IoNotifications from "svelte-icons-pack/io/IoNotifications";
import RiSystemArrowDropDownLine from "svelte-icons-pack/ri/RiSystemArrowDropDownLine";
import BsChatLeftDotsFill from "svelte-icons-pack/bs/BsChatLeftDotsFill";
import BiSolidWallet from "svelte-icons-pack/bi/BiSolidWallet";
import CgMenuCheese from "svelte-icons-pack/cg/CgMenuCheese";
import Navprofile from "../../profilecomponent/main/navprofile.svelte"
import Coins from "../../profilecomponent/main/coins.svelte"
import {
    createEventDispatcher
} from 'svelte'
const dispatch = createEventDispatcher()

import {
    UserProfileEl
} from "$lib/index";
import {
    profileStore
} from "$lib/store/profile";
import {
    onMount
} from "svelte";
const {
    profileEl,
    handleDefaultwallet
} = UserProfileEl()
import {
    browser
} from '$app/environment'
import {
    goto
} from "$app/navigation"
const id = browser && JSON.parse(localStorage.getItem('user'))
import {
    default_Wallet
} from "$lib/store/coins"

let isCoinDrop = false
const handleCoinsDrop = ((e) => {
    if (isCoinDrop) {
        isCoinDrop = false
    } else {
        isCoinDrop = true
    }
})

$: {
    id && onMount(async () => {
        profileEl()
        handleDefaultwallet()
    })
}

let userProfile = false
const handleUserProfile = (() => {
    if (userProfile) {
        userProfile = false
    } else {
        userProfile = true
    }
})

const handleCoinSelect = ((e) => {
    default_Wallet.set(e.detail)
    handleCoinsDrop()
})
const handleChat = ((e) => {
    dispatch("handleChatRoom", e)
})
</script>

<div class="sc-DtmNo euzHLF right">
    <div class="sc-gjNHFA juteh wallet-enter">
        <div class="sc-fmciRz LQlWw">
            <button on:click={()=>handleCoinsDrop("open")} class="sc-iFMAIt icGouR">
                <div class="sc-eXlEPa boxpOO">
                    <img class="coin-icon" alt="" src={id && $default_Wallet.coin_image}>
                    <span class="currency">{id && $default_Wallet.coin_name}</span>
                    <Icon src={RiSystemArrowDropDownLine}  size="18"  color="rgb(171, 182, 194)" className="custom-icon" title="arror" />
                </div>
                <div class="sc-Galmp erPQzq coin notranslate balance">
                    <div class="amount">
                        <span class="amount-str">{$default_Wallet.balance}.<span class="suffix">{$default_Wallet.suffix}</span></span>
                    </div>
                </div>
            </button>
            {#if isCoinDrop}
                <Coins on:coinDefault={handleCoinSelect} />
            {/if}
            <button on:click={()=> goto("/wallet/deposit")} class="sc-iqseJM sc-bqiRlB eWZHfu button button-normal sc-iqVWFU fGPfpD">
                <div class="button-inner">
                    <span class="wallet-icon">
                        <Icon src={BiSolidWallet}  size="18"  color="rgb(255, 255, 255)"  title="arror" />
                    </span>
                    <span style="padding-left: 10px;">Wallet</span>
                </div>
            </button>
        </div>
    </div>
    <div class="sc-gnnDb fWkueO">
        <div class="user-wrap">
            {#if $profileStore}
            <a href="/user/profile/505090">
             <img class="avatar" alt="" src={$profileStore.profile_image}>
            </a>
            {:else}
                <div class="center">
                    <div class="wave"></div>
                    <div class="wave"></div>
                    <div class="wave"></div>
                    <div class="wave"></div>
                    <div class="wave"></div>
                </div>
            {/if}
            <button on:mouseenter={handleUserProfile} on:mouseleave={handleUserProfile} class="svg">
                <span class="na-menu"><Icon src={CgMenuCheese}  size="18"   color="rgba(153, 164, 176, 0.6)" className="custom-icon" title="arror" /></span>
                {#if userProfile}
                    <Navprofile />
                {/if}
            </button>
        </div>
    </div>
    <button on:click={()=> goto("/chat")} class="sc-dcgwPl bbYXSv private-chat">
        <span class="nav-message"><Icon src={BsChatLeftDotsFill}  size="18"   color="rgba(153, 164, 176, 0.6)" className="custom-icon" title="arror" /></span>
    </button>
    <button  id="notice" class="sc-ksHpcM kultDa notice">
        <div class="notice-btn ">
            <span class="na-notification"><Icon src={IoNotifications}  size="18"   color="rgba(153, 164, 176, 0.6)" className="custom-icon" title="arror" /></span>
        </div>
    </button>
    <button on:click={handleChat} id="chat" class="sc-eicpiI PGOpB">
        <div class="chat-btn ">
            <img class="sc-gsDKAQ hxODWG icon" src="https://www.linkpicture.com/q/play_2.png" alt="" />
            <div class="sc-fotOHu gGSOuF badge ">26</div>
        </div>
    </button>
</div>

<style>

</style>