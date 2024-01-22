<template>
  <div id="app">
    <Header />
    <div class="board">
      <div class="line">
        <h2 class="tittle">BackLog</h2>
        <Container
          group-name="collum"
          @drag-start="handleDrag('backlog', $event)"
          @drop="handleDrop('backlog', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'drop'}"
        >
          <Draggable v-for="card in cards.backlog" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
      </div>
      <div class="line">
        <h2 class="tittle">In Progress</h2>
        <Container
          group-name="collum"
          @drag-start="handleDrag('inp', $event)"
          @drop="handleDrop('inp', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'drop'}"
        >
          <Draggable v-for="card in cards.inp" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
      </div>
      <div class="line">
        <h2 class="tittle">To do</h2>
        <Container
          group-name="collum"
          @drag-start="handleDrag('toDo', $event)"
          @drop="handleDrop('toDo', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'drop'}"
        >
          <Draggable v-for="card in cards.toDo" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
      </div>
      <div class="line">
        <h2 class="tittle">Review</h2>
        <Container
          group-name="collum"
          @drag-start="handleDrag('review', $event)"
          @drop="handleDrop('review', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'drop'}"
        >
          <Draggable v-for="card in cards.review" :key="card.id">
            <Card>
              {{ card.text }}
            </Card>
          </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Card from "./components/Card.vue";
import fakeApi from "./fakeApi";
import { Container, Draggable } from "vue3-smooth-dnd";

export default {
  name: "App",
  components: { Header, Card, Container, Draggable },
  data() {
    return {
      cards: {
        backlog: fakeApi.backlog,
        inp: fakeApi.inp,
        toDo: fakeApi.toDo,
        review: fakeApi.review,
      },
      draggingCard: {
        line: "",
        index: -1,
        cardData: {},
      },
    };
  },
  methods: {
    handleDrag(line, dragResult) {
      const { payload, isSource } = dragResult;
      if (isSource) {
        this.draggingCard = {
          line,
          index: payload.index,
          cardData: {
            ...this.cards[line][payload.index],
          },
        };
      }
    },
    handleDrop(line, dropResult) {
      const { removedIndex, addedIndex } = dropResult;

      if (line === this.draggingCard.line && removedIndex === addedIndex) {
        return;
      }

      if (removedIndex !== null) {
        this.cards[line].splice(removedIndex, 1);
      }

      if (addedIndex !== null) {
        this.cards[line].splice(addedIndex, 0, this.draggingCard.cardData);
      }
    },
    getChildPayload(index) {
      return {
        index,
      };
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

#app {
  font-family: "Poppins", sans-serif;
  color: #ebecf0;
  font-weight: 600;
  line-height: 20px;
}

body,
html {
  margin: 0;
  padding: 0;
  border: 0;
  background-color: #936424;
  font-size: 62.5%;
}

.board {
  display: flex;
  justify-content: space-between;
  margin: 0 20% 0 20%;
}

.line {
  background: black;
  width: 23rem;
  min-height: 20rem; 
  border-radius: 0.8rem;
  box-shadow: 0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
  margin: 1.2rem 0.8rem;
  padding: 0 0.7rem;
}

.tittle {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
}

.drop{
  background: #22272b;
  border-radius: 0.4rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}
</style>
