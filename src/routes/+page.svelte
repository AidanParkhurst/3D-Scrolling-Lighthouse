<script>
    import LightHouse from '$lib/assets/LightHouse.png';
    import MePhoto from '$lib/assets/menet.png';

    let contents;
    let scrolled = 0;
    
    // Rotate the light beams based on the scroll position
    $: angle = scrolled / 10;
    $: screen = Math.floor(angle / 180);

    // Move a spotlight across the screen in line with the beams
    let spotX;
    $:
    if(angle % 360 < 180)
        spotX = - (150 * Math.cos(angle * (Math.PI / 180)));
    else
        spotX = (150 * Math.cos(angle * (Math.PI / 180)));

    // Update z-index of the lighthouse to clarify which beam is in front
    let leftZ;
    let rightZ;
    $:
    if(angle % 180 < 90) {
        leftZ = 0;
        rightZ = 3;
    } else {
        leftZ = 3;
        rightZ = 0;
    }

    // Update the angle of the spotlight to follow the beam
    let spotAngle;
    $:
    if (angle % 360 < 180) {
        spotAngle = (angle+90) + 180;
    } else {
        spotAngle = (angle+90);
    }
</script>

<title>Aidan Parkhurst</title>
<div class="back">
    {#if angle < 90}
        <h1 class="title">Aidan Parkhurst</h1>
        <p class="title">Scroll to see more...</p>
    {/if}
    <img class="lighthouse left" src={LightHouse} style="z-index: {leftZ}" alt="Lighthouse">
    <img class="lighthouse right" src={LightHouse} style="z-index: {rightZ}"alt="Lighthouse">
    <div class="beams" style="transform:rotateY({angle}deg);"></div>
</div>
<div class="scrollable" bind:this={contents}
                        on:scroll={()=>scrolled=contents.scrollTop} >
    <div class="filler" style="min-height: 1000%">
    </div>
</div>
<div class="contents">
    <div class="spot" style="transform:translateX({spotX}%) rotateY({spotAngle}deg)">
        <div class="content">
            {#if screen==0}
                <h1>Hello! I'm Aidan Parkhurst</h1>
                <img src={MePhoto} alt="Me" style="width: 100%; border-radius: 15px; border: 4px solid #022840"/>
                <h2>I'm a 21 year old programmer, student, and artist based out of Boston.</h2>
            {:else if screen==1}
                <h1>I'm a Front End Developer</h1>
                <h2>and I strive to make beautiful, memorable websites</h2>
                <h2>I have experience making simple personal portfolios, as well as highly functional web applications</h2>
                <h2>oh, and I'm accepting freelance work at the moment!</h2>
                <a href="mailto:aidanp@rkhur.st">Interested? Get in touch!</a>
            {:else if screen==2}
                <h1>I'm a Game Developer</h1>
                <p>
                    Videogames have played a huge roll in my life, and I'm passionate about them as a tool for expression and a medium for art.
                    I've made a few that you can try right in your browser! (Only on computer, sorry mobile users!)
                </p>
                <a target="_blank" href='https://rkhur.st/Wave'>Play Wave</a>
                <a target="_blank" href='https://rkhur.st/Communion'>Play Communion</a>
            {:else if screen==3}
                <h1>And that's all! (For now)</h1>
                <h2>but if you wanna know more, or to plan a project of your own, you might be interested in...</h2>
                <a href="mailto:aidanp@rkhur.st">My email, aidanp@rkhur.st</a>
                <a href="https://github.com/AidanParkhurst">My code, on my github</a>
                <a href="https://www.instagram.com/aaaiiidaaan/">or My photos, on instagram</a>
            {/if}
        </div>
    </div>
</div>
<style>
    .back {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
        width: 100%;
        position: fixed;
        top: 0;
        left: 0;
        background-color: #022840;
        perspective: 100vw;
    }
    h1.title {
        position: absolute;
        font-family: 'Quicksand';
        color:#F2D750;
        transform: translateY(-33vh);
    }
    p.title {
        position: absolute;
        font-family: 'Quicksand';
        color:#F2D750;
        transform: translateY(-25vh);
    }
    .lighthouse.left {
        position: absolute;
        transform: scale(0.75) translateX(2%) translateY(35%);
        /* only show the left half */
        clip-path: polygon(0 0, 50% 0, 50% 100%, 0 100%);
    }
    .lighthouse.right {
        position: absolute;
        transform: scale(0.75) translateX(2%) translateY(35%);
        /* only show the right half */
        clip-path: polygon(50% 0, 100% 0, 100% 100%, 50% 100%);
    }
    .beams {
        width: 0;
        height: 0;
        border-top: 20vw solid transparent;
        border-bottom: 20vw solid transparent;
        border-right: 50vw solid #F2D750;
        border-left: 50vw solid #F2D750;
    } 

    /* container which detects scrolling anywhere on the page */
    .scrollable {
        height: 100%;
        width: 100%;
        position: absolute;
        top: 0;
        left: 0;
        overflow: scroll;
    }

    /* actual contents of the page */
    .contents {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
        width: 100%;
        position: fixed;
        top: 0;
        left: 0;
        pointer-events: none;
    }
    .spot {
        position: fixed;
        width: 60vw;
        height: 60vw;
        border-radius: 50%;
        background-color: #F2D750;
        z-index: 3;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .content {
        height: 80%;
        width: 55%;
        color: #022840;
        font-family: 'Quicksand';
        
        display: flex;
        flex-direction: column;
        justify-content: center;
        gap: 20px;
    }
    h2 {
        font-weight: 400;
    }
    p {
        font-size: larger;
    }
    a {
        pointer-events: auto;
        padding: 10px 20px;
        background-color: #022840;
        border: 2px solid #022840;
        color: #F2D750;
        font-size: larger;
        font-weight: bold;
        text-decoration: none;
        border-radius: 20px;
        transition: all 0.3s;
    }
    a:hover {
        background-color: #022840CC;
    }
    a:active {
        background-color: #F2D750;
        color: #022840;
    }

    /* make the light larger on mobile */
    @media (max-width: 600px) {
        .spot {
            width: 85vh;
            height: 85vh;
        }

        .beams {
            border-top: 20vw solid transparent;
            border-bottom: 20vw solid transparent;
            border-right: 62vw solid #F2D750;
            border-left: 62vw solid #F2D750;
        }
    }

</style>