<template>
    <div class="field-wrapper">
        <div class="cards">
            <card v-for="(card, index) in cards" :key="index" :card="card" :index="index" @click="onclick" />
        </div>
    </div>
</template>

<script>
    import Card from "@/components/Card";

    export default {
        name: 'Field',
        components: {
            Card
        },
        data() {
            return {
                set: ['ace', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'jack', 'queen', 'king'],
                suits: ['heart', 'diamond', 'spade', 'club'],
                cards: [],
                buffer: [],
                correctCount: 0
            };
        },
        methods: {
            onclick: function (index) {
                // Can't click on shown card or open 3 cards in the time
                if (!this.cards[index].closed || this.buffer.length === 2) {
                    return false;
                }
                this.buffer.push(index);
                if (this.buffer.length === 2) {
                    let firstCard = this.cards[this.buffer[0]].name + this.cards[this.buffer[0]].suit;
                    let secondCard = this.cards[this.buffer[1]].name + this.cards[this.buffer[1]].suit;
                    // Show the second card
                    this.cards[index].closed = false;
                    if (firstCard === secondCard) {
                        // Increment correct opened cards count
                        this.correctCount += 2;
                        // Clear cards buffer
                        this.buffer = [];
                    } else {
                        // Wrong cards
                        setTimeout(() => {
                            this.cards[this.buffer[0]].closed = true;
                            this.cards[this.buffer[1]].closed = true;
                            // Clear cards buffer
                            this.buffer = [];
                        }, 500);
                    }
                } else {
                    // Show the first card
                    this.cards[index].closed = false;
                }
            }
        },
        created() {
            // choose random cards with random suits from collection by data collections
            let cardIndex, suitIndex, key;
            let collection = [];
            let cardsCount = 24;
            let maxSuitIndex = this.suits.length - 1;
            let maxCardIndex = this.set.length - 1;
            while (this.cards.length < cardsCount) {
                cardIndex = Math.floor(Math.random() * maxCardIndex);
                suitIndex = Math.floor(Math.random() * maxSuitIndex);
                key = `${cardIndex}${suitIndex}`;
                if (collection.indexOf(key) === -1) {
                    this.cards.push(
                        {
                            name: this.set[cardIndex],
                            suit: this.suits[suitIndex],
                            closed: true
                        },
                        {
                            name: this.set[cardIndex],
                            suit: this.suits[suitIndex],
                            closed: true
                        }
                    );
                }
                collection.push(key);
            }
            if (this.cards.length > 0) {
                let j = 0;
                for (let i = this.cards.length - 1; i > 0; i--) {
                     j = Math.floor(Math.random() * (i + 1));
                     [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
                }
            }
        }
    }
</script>

<style scoped lang="sass">
    .cards
        display: flex
        flex-wrap: wrap
        align-items: center
        justify-content: center
        background-color: #057534
        box-shadow: inset 0 0 5px -1px #444
        border-radius: 5px
        margin: 10px 0
        height: calc(100vh - 20px)
        box-sizing: border-box
</style>