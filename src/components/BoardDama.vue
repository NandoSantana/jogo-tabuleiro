<template>
  <div class="board">
    <table>
      <tbody>
      <tr v-for="(row, rowIndex) in board" :key="rowIndex">

          <td v-for="(piece, colIndex) in row" class="cell" :key="colIndex">
            <!-- evento drag n drop-->
            <div
                @click="handleCellClick(rowIndex, colIndex, piece)"
                :class="{ selectable: isSelectable(rowIndex, colIndex) }"
            >
              {{ piece }}
            </div>
          </td>

      </tr>
      </tbody>
    </table>

  </div>
</template>

<script>
export default {
  name: "Board-dama",
  // props: ['board'],
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
      selectedPiece: null, // Armazena a peça selecionada
      currentPlayer: 'player1', // Define o jogador atual
      // Outras informações do jogo aqui
    };

  },
  methods: {
    handleCellClick(row, col) {
      const piece = this.board[row][col];
    console.log('peça',piece);
      // Verificar se a célula clicada contém uma peça do jogador atual
      if (piece === 'X' || piece === 'O') {
        // if (!this.selectedPiece) {
          // Se nenhuma peça estiver selecionada, selecione a peça clicada
          this.selectedPiece = { row, col };
          console.log(this.selectedPiece);

          // Caso contrário, verifique se o movimento é válido e mova a peça
          console.log('valid?',this.isValidMove(this.selectedPiece, row, col));
          if (this.isValidMove(this.selectedPiece, row, col)) {
            this.movePiece(this.selectedPiece, row, col);
            this.selectedPiece = null; // Desselecione a peça
          } else {
            // Se o movimento não for válido, mantenha a peça selecionada
            this.selectedPiece = { row, col };
          }

      } else if (this.selectedPiece) {
        // Se uma peça já estiver selecionada e o jogador clicar em uma célula vazia,
        // desselecione a peça
        this.selectedPiece = null;
      }
    },
    isSelectable(row, col) {
      // Verificar se a célula é selecionável (ou seja, se contém uma peça do jogador atual)
      if (!this.selectedPiece) {
        return this.board[row][col] === 'X'; // Substitua 'P' pelo jogador atual
      } else {
        return false; // Se uma peça já estiver selecionada, as outras células não são selecionáveis
      }
    },
    isSelectedPiece(row, col) {
      // Verifique se a posição atual é a peça selecionada
      return (
          this.selectedPiece &&
          this.selectedPiece.row === row &&
          this.selectedPiece.col === col
      );
    },
    isValidMove(selectedPiece, targetRow, targetCol) {
      // Implemente a lógica para verificar se o movimento é válido
      // Isso depende das regras do jogo de damas
      // Exemplo simples: permitir movimento diagonal para frente
      // para uma posição vazia
      console.log('is valid to move?', {targetRow, targetCol },selectedPiece.row, selectedPiece.col)
      // const piece = selectedPiece.piece;
      const currentRow = selectedPiece.row;
      const currentCol = selectedPiece.col;

      // Lógica simples para um movimento válido
      return (
          Math.abs(targetRow - currentRow) === 1 &&
          Math.abs(targetCol - currentCol) === 1 &&
          this.board[targetRow][targetCol] === null
      );
    },
    movePiece(selectedPiece, targetRow, targetCol) {
      console.log('tentando mover');
      // Implemente a lógica para mover a peça no tabuleiro
      // Atualize a matriz board com o novo estado do jogo
      // Isso também depende das regras do jogo de damas
      // Exemplo simples: apenas atualize a matriz com a nova posição
      const piece = selectedPiece.piece;
      const currentRow = selectedPiece.row;
      const currentCol = selectedPiece.col;

      // Remova a peça da posição atual
      this.board[currentRow][currentCol] = null;

      // Coloque a peça na nova posição
      this.board[targetRow][targetCol] = piece;
    },

  },
  // Métodos para atualizar o estado do jogo
};
</script>

<style scoped>
/* Estilos CSS para o tabuleiro */
.board {
  /* Estilize o tabuleiro de acordo com suas preferências */
}
.selectable {
  background-color: yellow;
}
.cell {
  width: 50px;
  height: 50px;
  background-color: white;
  border: 1px solid #000;
  /* Estilize as casas do tabuleiro de acordo com suas preferências */
}
</style>
