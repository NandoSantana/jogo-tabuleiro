<template>
  <div class="board">
    <table>
      <tbody>
      Vez de {{currentPlayer}} <br/>-  Jogadas X = {{playerXScore}} <br/>- Jogadas O = {{playerOScore}}
      <tr v-for="(row, rowIndex) in board" :key="rowIndex">
          <td v-for="(piece, colIndex) in row"
              class="cell"
              :key="colIndex"
              @click="handleCellClick(rowIndex, colIndex, piece)"
              :class="{
                selectable: isSelectedPiece(rowIndex, colIndex),
                 piece: true, // Classe básica para todas as células
                // selectable: isSelectable(rowIndex, colIndex), // Para células selecionáveis
                empty: piece === null, // Para células vazias
                player1: piece === 'X', // Para peças do jogador 1
                player2: piece === 'O' // Para peças do jogador 2
              }">
            <!-- evento drag n drop-->
            <div
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
  data() {
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

    return {
      board: board, // Matriz que representa o tabuleiro
      selectedPiece: null, // Armazena a peça selecionada
      currentPlayer: 'X', // Define o jogador atual
      lastOpponent: 'O',
      validMoves: [],
      selectedCell: null,
      playerXScore: 0,
      playerOScore: 0,
      capturado: null,
      // Outras informações do jogo aqui
    };

  },
  methods: {
    handleCellClick(row, col) {
      const piece = this.board[row][col];

      if (piece === this.currentPlayer) {

        // Se o jogador clicou em sua própria peça, selecione-a
        this.selectedCell = { row, col, piece };
        this.selectedPiece = { row, col, piece };
        console.log('Selected', this.selectedPiece)
        // Continue com o movimento da peça como anteriormente
        // this.movePiece(this.selectedCell, row, col);
        // this.selectedCell = null;
      } else if ( this.selectedPiece ) {
        // Verifique se o movimento é válido e realize a captura de peças
          this.movePiece(this.selectedPiece, row, col);
          this.capturePiece(row, col);

          // Continue com o movimento da peça como anteriormente
          this.selectedPiece = null;
          // console.log('capturado', this.capturado)
          // Alterne entre os jogadores
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    capture(fromRow, fromCol, toRow, toCol) {
      const opponent = this.currentPlayer === 'X' ? 'O' : 'X';

      // Verifique se a célula de destino está vazia
      if (this.board[toRow][toCol] === null) {
        // Verifique se há uma peça do oponente na célula intermediária
        const middleRow = (fromRow + toRow) / 2;
        const middleCol = (fromCol + toCol) / 2;

        if (this.board[middleRow][middleCol] === opponent) {
          // Realize a captura de peça:
          // 1. Limpe a célula de origem
          this.board[fromRow][fromCol] = null;

          // 2. Limpe a célula do oponente
          this.board[middleRow][middleCol] = null;

          // 3. Mova a peça para a célula de destino
          this.board[toRow][toCol] = this.currentPlayer;

          // Atualize a pontuação do jogador
          if (this.currentPlayer === 'X') {
            this.playerXScore++;
          } else {
            this.playerOScore++;
          }

          // Alterne entre os jogadores
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      }
    },
    // capturePiece(selectedPiece, targetRow, targetCol) {
    //   // Realize a captura de peças (remova a peça do adversário)
    //   // console.log(this.board[selectedPiece.row][selectedPiece.col].piece);
    //   // const piece = this.board[selectedCell.row][selectedCell.col].piece;
    //   const opponent = this.currentPlayer === 'X' ? 'O' : 'X';
    //   console.log('choose', {targetRow, targetCol});
    //   console.log('board', this.board);
    //
    //   // console.log('opponent',opponent)
    //   if (selectedPiece.piece !== opponent) {
    //     console.log('engoliu linha',  this.board[targetRow]);
    //     console.log('engoliu coluna',  targetCol);
    //
    //     this.board[targetRow].forEach( el => {
    //       console.log(el);
    //       this.board[targetRow][targetCol] = selectedPiece.piece;
    //     });
    //     console.log('engoliu',  this.board[targetRow[targetCol]]);
    //     // this.board[targetRow][targetCol] = selectedPiece.piece;
    //     this.capturado = {targetRow, targetCol};
    //
    //     if (this.currentPlayer === 'O') {
    //       this.playerXScore++;
    //     } else {
    //       this.playerOScore++;
    //     }
    //
    //     // return this.board[targetRow][targetCol];
    //   }
    // },

    capturePiece(row, col) {
      // Verifique se o jogador atual é 'X' ou 'O' e identifique o oponente
      const currentPlayer = this.currentPlayer;
      console.log('currentPlayer', currentPlayer)
      const opponent = currentPlayer === 'X' ? 'O' : 'X';

      // Calcule a posição da célula intermediária
      const middleRow = Math.round((this.selectedPiece.row + row) / 2);
      const middleCol = Math.round((this.selectedPiece.col + col) / 2);

      // Verifique se a célula intermediária contém uma peça do oponente

      if ( this.board[this.selectedPiece.row][this.selectedPiece.col] !== opponent ) {

        // Limpe a célula intermediária (peça do oponente capturada)
        this.board[this.selectedPiece.row][this.selectedPiece.col] = null;
        this.board[middleRow][middleCol] = null;

        // Mova a peça para a célula de destino
        this.board[row][col] = currentPlayer;

        // Atualize a pontuação do jogador atual
        if (currentPlayer === 'X') {
          this.playerXScore++;
        } else {
          this.playerOScore++;
        }


      }
    },

    calculateValidMoves(selectedPiece) {
      this.validMoves.pop();
      const validMoves = [];
      // const currentPlayer = selectedPiece.player;
      const currentRow = selectedPiece.row;
      const currentCol = selectedPiece.col;

      // Verifique as células diagonais adjacentes
      // Adicione-as à lista de movimentos válidos se forem válidas
      const directions = [
        { row: -1, col: -1 },
        { row: -1, col: 1 },
        // Adicione mais direções de acordo com as regras do seu jogo
      ];

      for (const dir of directions) {
        const newRow = currentRow + dir.row;
        const newCol = currentCol + dir.col;

        // Verifique se a nova posição está dentro do tabuleiro
        if (newRow >= 0 && newRow < this.board.length && newCol >= 0 && newCol < this.board[0].length) {
          // const cell = this.board[newRow][newCol];

          // Verifique se a célula está vazia e se o movimento é válido
          // if (!cell && this.isValidMove(currentPlayer, currentRow, currentCol, newRow, newCol)) {
            validMoves.push({ row: newRow, col: newCol });
          // }
        }
      }

      this.validMoves = validMoves;

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

      // this.calculateValidMoves(this.selectedPiece)
      return (
          this.selectedPiece &&
          this.selectedPiece.row === row &&
          this.selectedPiece.col === col
      );
    },
    isSelectoGo() {
      // Verifique se a posição atual é a peça selecionada

      this.calculateValidMoves(this.selectedPiece)
      return true;
    },
    isValidMove(selectedPiece, targetRow, targetCol) {
      const currentRow = selectedPiece.row;
      const currentCol = selectedPiece.col;
      const piece = this.board[currentRow][currentCol];
      // Verifique se o movimento é diagonal para frente (depende do jogador atual)
      const isForwardMove = piece === 'X'
          ? targetRow > currentRow // Para o jogador 'X', movimento para linhas maiores
          : targetRow < currentRow; // Para o jogador 'O', movimento para linhas menores

      // Verifique se o movimento é diagonal (diferença nas linhas e colunas é 1)
      const isDiagonalMove = Math.abs(targetRow - currentRow) === 1 &&
          Math.abs(targetCol - currentCol) === 1;

      // Verifique se a célula de destino está vazia
      const isTargetEmpty = this.board[targetRow][targetCol] === null;

      // Verifique se todas as condições são atendidas para um movimento válido
      return isForwardMove && isDiagonalMove && isTargetEmpty;
    },
    movePiece(selectedPiece, targetRow, targetCol) {
      // Implemente a lógica para mover a peça no tabuleiro
      // Atualize a matriz board com o novo estado do jogo
      // Isso também depende das regras do jogo de damas
      // Exemplo simples: apenas atualize a matriz com a nova posição
      this.capturado = {targetRow, targetCol};

      if(this.isValidMove(selectedPiece, targetRow, targetCol)){
        const piece = selectedPiece.piece;
        const currentRow = selectedPiece.row;
        const currentCol = selectedPiece.col;

        // Remova a peça da posição atual
        this.board[currentRow][currentCol] = null;

        // Coloque a peça na nova posição
        this.board[targetRow][targetCol] = piece;
        // return true
      }
      // return false;

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
.toGo{
  background-color: #42b983;
}

/* Estilos básicos para todas as células */
.cell {
  width: 50px;
  height: 50px;
  text-align: center;
  vertical-align: middle;
  font-size: 18px;
  border: 1px solid #000;
}

/* Estilo para células selecionáveis (por exemplo, movimentos válidos) */
.selectable {
  background-color: yellow;
}

/* Estilo para células vazias */
.empty {
  background-color: #fff;
}

/* Estilo para peças do jogador 1 (por exemplo, peças pretas) */
.player1 {
  background-color: #000;
  color: #fff;
}

/* Estilo para peças do jogador 2 (por exemplo, peças brancas) */
.player2 {
  background-color: yellowgreen;
  color: #000;
}

</style>
