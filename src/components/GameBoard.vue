<template>
    <div>
        <div class="game-board" :style="gameBoardStyle">
            <Card
                v-for="(card, index) in cards"
                :key="index"
                :image-src="card.imageSrc"
                :image-alt="card.imageAlt"
                :is-flipped="card.isFlipped"
                @card-clicked="handleCardClick(index)"
            />
        </div>
        <button @click="resetGame" class="reset-button">Reset Game</button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Card from "@/components/GameCard.vue";

import one from '@/assets/images/one.jpg';
import two from '@/assets/images/two.jpg';
import three from '@/assets/images/three.jpg';
import four from '@/assets/images/four.jpg';
import five from '@/assets/images/five.jpg';
import six from '@/assets/images/six.jpg';
import seven from '@/assets/images/seven.jpg';
import eight from '@/assets/images/eight.jpg';
import nine from '@/assets/images/nine.jpg';

const CARD_FLIP_DELAY = 1000;
const GAME_RESET_DELAY = 1000;

export default defineComponent({
    components: {
        Card,
    },
    props: {
        boardSize: {
            type: Number,
            default: 4,
        },
    },
    computed: {
        gameBoardStyle(): { gridTemplateColumns: string } {
            return {
                gridTemplateColumns: `repeat(${this.currentBoardSize}, minmax(100px, 1fr))`,
            };
        },
    },
    watch: {
        boardSize() {
            this.resetGame();
        },
    },

    data(): {
        cards: {
            imageSrc: string;
            imageAlt: string;
            isFlipped: boolean;
            isMatched: boolean;
        }[];
        firstCard: number | null;
        secondCard: number | null;
        attempts: number;
        matchedPairs: number;
        currentBoardSize: number;
    } {
        return {
            cards: [],
            firstCard: null,
            secondCard: null,
            attempts: 0,
            matchedPairs: 0,
            currentBoardSize: this.boardSize,
        };
    },
    
    methods: {
        
        shuffleCards() {
            const images = [one, two, three, four, five, six, seven, eight, nine, one, two, three, four, five, six, seven, eight, nine].slice(0, (this.boardSize * this.boardSize) / 2);

            const cardPairs = images.concat(images);

            for (let i = cardPairs.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [cardPairs[i], cardPairs[j]] = [cardPairs[j], cardPairs[i]];
            }

            this.cards = cardPairs.map(image => ({
                imageSrc: image,
                imageAlt: "Image",
                isFlipped: false,
                isMatched: false,
            }));
        },

        handleCardClick(index: number) {
            if (
                this.cards[index].isFlipped ||
                this.cards[index].isMatched ||
                this.secondCard !== null
            ) {
                return;
            }

            this.cards[index].isFlipped = true;

            if (!this.firstCard && this.firstCard !== 0) {
                this.firstCard = index;
            } else {
                this.secondCard = index;
                this.attempts++;
                console.log("Attempt #", this.attempts);

                setTimeout(() => {
                    this.checkMatch();
                }, CARD_FLIP_DELAY);
            }
        },

        checkMatch() {
            if (this.firstCard === null || this.secondCard === null) {
                return
            }

            if (this.cards[this.firstCard].imageSrc === this.cards[this.secondCard].imageSrc) {
                this.cards[this.firstCard].isMatched = true;
                this.cards[this.secondCard].isMatched = true;
                this.matchedPairs++;

                if (this.matchedPairs === this.cards.length / 2) {
                    alert(`You won! Attempts: ${this.attempts}`);
                    setTimeout(() => {
                        this.resetGame();
                    }, GAME_RESET_DELAY);
                }
            } else {
                this.cards[this.firstCard].isFlipped = false;
                this.cards[this.secondCard].isFlipped = false;
            }
            
            this.firstCard = null;
            this.secondCard = null;
        },

        resetGame() {
            let timeout = 0;
            if (this.attempts !== 0) {
                timeout = 500;
            }

            this.cards.forEach((card) => {
                if (card.isFlipped) {
                    card.isFlipped = false;
                }
            });

            setTimeout(() => {
                this.currentBoardSize = this.boardSize;
                this.cards = [];
                this.firstCard = null;
                this.secondCard = null;
                this.attempts = 0;
                this.matchedPairs = 0;
                this.shuffleCards();
            }, timeout);
        },
    },

    created() {
        this.shuffleCards();
    },
});
</script>

<style scoped>
.game-board {
    background-color: #ad159e;
    display: grid;
    grid-gap: 10px;
    max-width: 700px;
    margin: 0 auto;
    padding: 10px;
}
.reset-button {
  display: block;
  margin: 20px auto;
  padding: 10px 20px;
  background-color: #2c3e50;
  color: #fff;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.reset-button:hover {
  background-color: #1a2533;
}
</style>
