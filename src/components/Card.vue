<template>
    <div class="card-wrapper">
        <transition name="fade" mode="out-in" @leave="leave" @before-enter="beforeEnter" @enter="enter" :css="false">
            <div v-if="card.closed" :class="classes" @click="$emit('click', index)" :key="index + 'closed'"></div>
            <div v-else :class="classes" @click="$emit('click', index)" :key="index + 'open'"></div>
        </transition>
    </div>
</template>

<style scoped lang="sass">
    $cards: 'ace', 2, 3, 4, 5, 6, 7, 8, 9, 10, 'jack', 'queen', 'king'
    $suits: 'heart', 'diamond', 'club', 'spade'
    $cardWidth: 77
    $cardHeight: 117
    $heartMult: 0
    $diamondMult: 1
    $clubMult: 2
    $spadeMult: 3
    $x: 0
    $y: 0

    .card-wrapper
        width: 16.66%
        font-size: 0
        text-align: center
        margin: 10px 0
        position: relative
    .back
        background-position: -1001px 0px
    .card
        background-image: url('~@/assets/images/cards.png')
        background-repeat: no-repeat
        width: $cardWidth + px
        height: $cardHeight + px
        display: inline-block
        box-sizing: content-box
    .clickable
        cursor: pointer

    /* Cards class generator */
    @for $i from 1 through length($cards)
        $card: nth($cards, $i)
        @each $suit in $suits
            .card-#{$card}-#{$suit}
                // Position X of first cards
                @if ($i == 1)
                    $x: 0
                // other cards
                @else
                    $x: (($i - 1) * $cardWidth)

                // Position Y of cards
                @if ($suit == 'heart')
                    $y: $heartMult
                @else if ($suit == 'diamond')
                    $y: $diamondMult * $cardHeight
                @else if ($suit == 'spade')
                    $y: $spadeMult * $cardHeight
                @else if ($suit == 'club')
                    $y: $clubMult * $cardHeight

                background-position: -#{$x}px -#{$y}px
</style>

<script>
    export default {
        name: "Card",
        props: {
            card: Object,
            index: Number
        },
        computed: {
            /* Computing card classes */
            classes() {
                let classname, clickable;
                if (this.card.closed) {
                    classname = 'back';
                    clickable = true;
                } else {
                    classname = `card-${this.card.name}-${this.card.suit}`;
                    clickable = false;
                }
                return {
                    card: true,
                    [classname]: true,
                    clickable: clickable
                };
            }
        },
        methods: {
            leave(el, done) {
                let x = 1;
                const interval = setInterval(function() {
                    el.style.transform = `scaleX(${x})`;
                    x -= 0.05;
                    if (x <= 0) {
                        clearInterval(interval);
                        done();
                    }
                }, 10);
            },
            beforeEnter(el) {
                el.style.transform = 'scaleX(0)';
            },
            enter(el, done) {
                let x = 0;
                const interval = setInterval(function() {
                    el.style.transform = `scaleX(${x})`;
                    x += 0.05;
                    if (x >= 1) {
                        clearInterval(interval);
                        done();
                    }
                }, 10);
            }
        }
    }
</script>