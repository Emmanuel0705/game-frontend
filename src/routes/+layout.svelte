<script>
/** @type {import('./$types').PageLoad} */
export let data;
import { routes } from "../lib/store/routes"
$: routes.set(data)
import Statistics from "../lib/statistics/main/index.svelte";
import Navbar from "$lib/navbar.svelte";
import SideBar from "$lib/sideBar.svelte";
import Footer from "$lib/footer.svelte";
import Menubar from "$lib/mobile/menu/menubar.svelte";
import ChatSide from "../lib/chat-room/index.svelte"
import Notification from "../lib/notification/index.svelte";
import { handleisLoggin, handleisLoading } from "$lib/store/profile"
import "../styles/errors/error.css"
import {
    getAuth,
    onAuthStateChanged
} from "firebase/auth";
import {
    app
} from "$lib/firebaseAuth/index"
import {
    browser
} from '$app/environment'
import {
    onMount
} from "svelte";
import Closesidebar from "$lib/closesidebar.svelte";
import { statisticsEl} from "../lib/store/statistic"

let isOpenSide = true
let isChatRoom = 0
let isMenu = false
let sideDetection = 0

$:{
    onMount(() => {
    const auth = getAuth(app);
    onAuthStateChanged(auth, (user) => {
        if (user) {
            const uid = user.uid;
            handleisLoggin.set(true)
            handleisLoading.set(false)
        } else {
            handleisLoggin.set(false)
            handleisLoading.set(false)
        }
    });
})

}

let ens = browser && window.innerWidth

onMount(() => {
    if (browser && window.innerWidth < 650) {
        isOpenSide = true
        sideDetection = 0
    } else if (browser && window.innerWidth > 1000) {
        isOpenSide = true
        sideDetection = 240
    } else {
        isOpenSide = false
        sideDetection = 76
    }
})

const handleMainMenu = (() => {
    if (isOpenSide) {
        isOpenSide = false
        sideDetection = 76
    } else {
        isOpenSide = true
        sideDetection = 240
    }
})

browser && addEventListener("resize", () => {
    ens = (window.innerWidth)
})
let isnotification = false
const handleChatroom = ((e) => {
    if (isChatRoom) {
        isnotification = false
        isChatRoom = 0
    } else {
        isChatRoom = 360
        if (e === "notification") {
            console.log(e)
            isnotification = true
        } else {
            isnotification = false
        }
    }
})

const handleMenu = () => {
    if (isMenu) {
        isMenu = false
    } else {
        isMenu = true
    }
}

const handleStatistics = (() => {
    statisticsEl.set(false)
})
</script>

<div class="app">
    {#if $statisticsEl}
    <Statistics on:discloseStatistics={handleStatistics} />
    {/if}

    {#if (isOpenSide) }
    <div id="main" style={`width:${isOpenSide ? 240 : 76}px`}>
        <SideBar routes={data} styls={isOpenSide} />
    </div>
    {:else}
    <div id="main" style={`width:${isOpenSide ? 240 : 76}px`}>
        <Closesidebar routes={data} styls={isOpenSide} />
    </div>
    {/if}
    <div id="main">
        <div id="menu">
            <button style={`left:${isOpenSide ? 224 : 60}px`} on:click={handleMainMenu}  class="menu">
                <img src='https://www.linkpicture.com/q/menu_2.svg' alt="" />
            </button>
        </div>
    </div>

    <!-- ======================  mobile menu bar ================= -->
    {#if (isMenu)}
    <div class="menubar">
        <Menubar  on:menu={handleMenu}   />
    </div>
    {/if}
    <div id="right-bar" style={`width: ${isChatRoom ? ((ens - sideDetection) - 360) : ens - sideDetection}px;`} >
        <header>
            <Navbar on:handleMenuMobile={handleMenu} on:handleChatRoom={handleChatroom} styles={isOpenSide} chatroom={isChatRoom} />
        </header>

        {#if $handleisLoading}
        <!-- Loading animation -->
        <div class="center">
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
            <div class="wave"></div>
        </div>
        {:else}
        <main class="sc-lhMiDA ePAxUv">
            <slot></slot>
        </main>
        <footer>
            <Footer />
        </footer>
        {/if}

    </div>
    {#if (isChatRoom)}
    {#if isnotification}
    <Notification />
    {:else}
    <ChatSide on:closeChat={handleChatroom} />
    {/if}

    {/if}
</div>
