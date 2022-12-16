<template>
  <div class="game-board">
    <div v-for="(card, index) in cards" :key="index" @click="flipCard(index)" class="card">
      <img v-if="card.faceUp" :src="card.imageUrl" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      cards: []
    };
  },
  created() {
    this.fetchImages();
  },
  methods: {
    async fetchImages() {
      const response = await axios.get('https://api.unsplash.com/photos/random', {
        params: {
          count: 8,
          client_id: 'your_client_id'
        }
      });
      const images = response.data;
      this.cards = images.map(image => ({
        imageUrl: image.urls.small,
        faceUp: false
      }));
    },
    flipCard(index) {
      this.cards[index].faceUp = !this.cards[index].faceUp;
      const selectedCards = this.cards.filter(card => card.faceUp);
      if (selectedCards.length === 2) {
        this.checkForMatch(selectedCards[0], selectedCards[1]);
      }
    },
    checkForMatch(card1, card2) {
      if (card1.imageUrl === card2.imageUrl) {
        // Display a message if the cards match
      } else {
        // Flip the cards back over if they do not match
        setTimeout(() => {
          card1.faceUp = false;
          card2.faceUp = false;
        }, 1000);
      }
    }
  }
};
</script>
