<template>
    <div id="desc">
        <h1>How does it work?</h1>
        <p id="descBig" class="description">Here you can communicate with morse code by entering text in the box below and other users on the website with a device that can
            vibrate will receive a morse equivalent as a vibration sequence. <br>
            On the other hand, people with compatible smartphone may tap morse code on their screen to communicate.
        </p>
        <p id="descSmol" class="description"> To communicate with other users, use morse code by tapping the zone below.</p>
    </div>
</template>

<style scoped>
    .description{
        text-align: justify;
    }
    h1{
            font-size: 150%;
        }
    @media (min-width: 415px) {
        

        div{
            font-size: 120%;
        }

        #descSmol{
            display: none;
        }
    }

    @media (max-width: 414px) {
        #descBig{
            display: none;
        }
    }
</style>