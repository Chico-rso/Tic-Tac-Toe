<template>
  <div class="gameStatus" :class="gameStatusColor">
    {{ gameStatusMessage }}
    <table class="grid">
      <tr>
        <cell name="1"></cell>
        <cell name="2"></cell>
        <cell name="3"></cell>
      </tr>
      <tr>
        <cell name="4"></cell>
        <cell name="5"></cell>
        <cell name="6"></cell>
      </tr>
      <tr>
        <cell name="7"></cell>
        <cell name="8"></cell>
        <cell name="9"></cell>
      </tr>
    </table>
  </div>
</template>

<script>
import Cell from "./Cell.vue";
export default {
  components: { Cell },
  data() {
    return {
      activePlayer: "X",
      gameStatus: "turn",
      winPlayer: "",
      // gameStatusMessage: `X's turn`,
      // gameStatusColor: "statusTurn",
      moves: 0,
      cells: {
        1: "",
        2: "",
        3: "",
        4: "",
        5: "",
        6: "",
        7: "",
        8: "",
        9: "",
      },
      winConditions: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7],
      ],
    };
  },
  created() {
    Event.$on("strike", (cellNumber) => {
      this.cells[cellNumber] = this.activePlayer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      this.changePlayer();
    });
    Event.$on("gridReset", () => {
      Object.assign(this.$data, this.$options.data());
    });
  },
  computed: {
    nonActivePlayer() {
      if (this.activePlayer === "O") {
        return "X";
      }
      return "O";
    },
    gameStatusMessage() {
      if (this.gameStatus === "win") {
        return `${this.winPlayer}'s win!`;
      } else if (this.gameStatus === "draw") {
        return "Draw !";
      }
      return `${this.activePlayer}'s turn`;
    },
    gameStatusColor() {
      if (this.gameStatus === "draw") {
        return "statusDraw";
      } else if (this.gameStatus === "win") {
        return "statusWin";
      }
      return "statusTurn";
    },
  },
  watch: {
    // для изменения статусов сообщения и цвета
    // gameStatus() {
    //   if (this.gameStatus === "win") {
    //     this.gameStatusColor = "statusWin";
    //     return;
    //   } else if (this.gameStatus === "draw") {
    //     this.gameStatusColor = "statusDraw";
    //     this.gameStatusMessage = "Draw !";
    //     return;
    //   }
    //   this.gameStatusMessage = `${this.activePlayer}'s turn`;
    // },
  },

  methods: {
    changePlayer() {
      this.activePlayer = this.nonActivePlayer;
    },
    changeGameStatus() {
      if (this.checkForWin()) {
        return this.gameIsWon();
      } else if (this.moves === 9) {
        return "draw";
      }
      return "turn";
    },
    checkForWin() {
      for (let i = 0; i < this.winConditions.length; i++) {
        let wc = this.winConditions[i];
        let cells = this.cells;
        if (this.areEqual(cells[wc[0]], cells[wc[1]], cells[wc[2]])) {
          return true;
        }
      }
      return false;
    },
    areEqual() {
      var len = arguments.length;
      for (var i = 1; i < len; i++) {
        if (arguments[i] === "" || arguments[i] !== arguments[i - 1])
          return false;
      }
      return true;
    },
    gameIsWon() {
      Event.$emit("win", this.activePlayer);
      this.winPlayer = this.activePlayer;
      Event.$emit("freeze");
      return "win";
    },
  },
};
</script>

<style>
.grid {
  background-color: #34495e;
  color: #fff;
  width: 100%;
  border-collapse: collapse;
}
.gameStatus {
  margin: 0px;
  padding: 15px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background-color: blueviolet;
  color: #fff;
  font-size: 1.4em;
  font-weight: bold;
}
.statusTurn {
  background-color: rgb(56, 117, 119);
}
.statusWin {
  background-color: crimson;
}
.statusDraw {
  background-color: darkgoldenrod;
}
</style>
