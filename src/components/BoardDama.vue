<template>
  <div class="board">
    <table>
      <tbody>
      <tr v-for="(row, rowIndex) in board" :key="rowIndex">
        <td v-for="(piece, colIndex) in row" :key="colIndex">
          <!-- Verificar se a peça pertence ao "Player 2" -->
          <div v-if="piece === 'O'">O</div>
          <!-- Se não pertencer ao "Player 2", exiba outra coisa ou deixe em branco -->
          <div v-else>{{ piece }}</div>
        </td>
      </tr>
      </tbody>
    </table>
<!--    <div v-for="row in 8" :key="row" class="row">-->

<!--      <div v-for="col in 8" :key="col" class="cell">-->
<!--      {{board[row][col]}}-->

<!--&lt;!&ndash;        &lt;!&ndash; Renderizar peças com base no estado do jogo &ndash;&gt;&ndash;&gt;-->
<!--&lt;!&ndash;        <div class="piece" v-if="board[row][col] === 'Player 2'">2</div>&ndash;&gt;-->
<!--&lt;!&ndash;        <div class="piece" v-if="board[row][col] === 'Player 1'">1</div>&ndash;&gt;-->
<!--      </div>-->
<!--    </div>-->
  </div>
</template>

<script>
export default {
  name: "Board-dama",
  data() {
    // const board = [
    //   [null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }],
    //   [{ piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null],
    //   [null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }, null, { piece: 'B', player: 'Player 1' }],
    //   [null, null, null, null, null, null, null, null],
    //   [null, null, null, null, null, null, null, null],
    //   [{ piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null],
    //   [null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }],
    //   [{ piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null, { piece: 'W', player: 'Player 2' }, null],
    // ];

    // const board = [
    //   [null, 'B', null, 'B', null, 'B', null, 'B'],
    //   ['B', null, 'B', null, 'B', null, 'B', null],
    //   [null, 'B', null, 'B', null, 'B', null, 'B'],
    //   ['', null, '', null, '', null, '', null],
    //   [null, '', null, '', null, '', null, ''],
    //   ['W', null, 'W', null, 'W', null, 'W', null],
    //   [null, 'W', null, 'W', null, 'W', null, 'W'],
    //   ['W', null, 'W', null, 'W', null, 'W', null],
    // ];

    const board = [
      ['O', null, 'O', null, 'O', null, 'O', null],
      [null, 'O', null, 'O', null, 'O', null, 'O'],
      ['O', null, 'O', null, 'O', null, 'O', null],
      [null, null, null, null, null, null, null, null],
      [null, null, null, null, null, null, null, null],
      [null, 'X', null, 'X', null, 'X', null, 'X'],
      ['X', null, 'X', null, 'X', null, 'X', null],
      [null, 'X', null, 'X', null, 'X', null, 'X'],
    ];
    // Exemplo de como validar se uma peça pertence ao "Player 2" em uma posição específica (row, col)
    // const row = 6; // Exemplo: verificar a peça na linha 6
    // const col = 1; // Exemplo: verificar a peça na coluna 1

    // if (board[row][col] && board[row][col].player === 'Player 2') {
    //   // A peça na posição (row, col) pertence ao "Player 2"
    //   console.log('Esta peça pertence ao Player 2.');
    // } else {
    //   // A peça não pertence ao "Player 2" ou a posição está vazia
    //   console.log('Esta peça não pertence ao Player 2 ou a posição está vazia.');
    // }

    return {
      board: board, // Matriz que representa o tabuleiro
      currentPlayer: 'player1', // Define o jogador atual
      // Outras informações do jogo aqui
    };
  },
  // Métodos para atualizar o estado do jogo
};
</script>

<style scoped>
/* Estilos CSS para o tabuleiro */
.board {
  /* Estilize o tabuleiro de acordo com suas preferências */
}

.cell {
  width: 50px;
  height: 50px;
  background-color: white;
  border: 1px solid #FFF;
  /* Estilize as casas do tabuleiro de acordo com suas preferências */
}
</style>
