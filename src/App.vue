<template>
  <div id="app">
    <Header />
    <div class="board">
      <div
        class="line"
        v-for="(marcacaoKey, index) in Object.keys(marcacoes)"
        :key="index"
      >
        <h2 class="tittle">{{ marcacaoKey }}</h2>
        <Container
          group-name="column"
          @drag-start="handleDrag(marcacaoKey, $event)"
          @drop="handleDrop(marcacaoKey, $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{ className: 'drop' }"
        >
          <Draggable
            v-for="conversa in marcacoes[marcacaoKey].conversas"
            :key="conversa.id"
          >
            <Card>
              {{ conversa.name }}
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
import fakeApiConnect from "./fakeApiConnect";
import { Container, Draggable } from "vue3-smooth-dnd";

export default {
  name: "App",
  components: { Header, Card, Container, Draggable },
  data() {
    return {
      marcacoes: fakeApiConnect.marcacoes,
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
            ...this.marcacoes[line].conversas[payload.index],
          },
        };
      }
    },
    handleDrop(line, dropResult) {
      const { removedIndex, addedIndex } = dropResult;
      console.log(this.marcacoes[line])

      if (line === this.draggingCard.line && removedIndex === addedIndex) {
        return;
      }

      if (removedIndex !== null) {
        this.marcacoes[line].conversas.splice(removedIndex, 1);
      }

      if (addedIndex !== null) {
        this.marcacoes[line].conversas.splice(addedIndex, 0, this.draggingCard.cardData);
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
  margin: 0 10% 0 10%;
}

.line {
  background: black;
  width: 40rem;
  min-height: 40rem;
  border-radius: 0.8rem;
  box-shadow: 0 0.1rem 0.2rem 0 rgba(33, 33, 33, 0.1);
  margin: 1.2rem 0.8rem;
  padding: 0 0.7rem;
}

.tittle {
  padding: 0.8rem 0.5rem;
  margin-bottom: 0.6rem;
  font-size: 14px;
}

.drop {
  background: #22272b;
  border-radius: 0.4rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}
</style>