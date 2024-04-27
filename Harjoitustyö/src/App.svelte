<script>
  import { onMount } from 'svelte';

  // Pelialusta mallia 3x3
  let board = [
    [null, null, null],
    [null, null, null],
    [null, null, null],
  ];

  let currentPlayer = 'X';
  let winner = null;

  // Tapahtuma joka tarkastaa voiton ja vaihtaa pelaajaa siirron jälkeen
  const makeMove = (row, col) => {
    if (!board[row][col] && !winner) {
      board[row][col] = currentPlayer;
      checkWinner();
      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    }
  };

  // Tarkastaa voittajan
  const checkWinner = () => {
    for (let i = 0; i < 3; i++) {
      if (
        board[i][0] &&
        board[i][0] === board[i][1] &&
        board[i][0] === board[i][2]
      ) {
        winner = board[i][0];
        return;
      }
      if (
        board[0][i] &&
        board[0][i] === board[1][i] &&
        board[0][i] === board[2][i]
      ) {
        winner = board[0][i];
        return;
      }
    }

    if (
      board[0][0] &&
      board[0][0] === board[1][1] &&
      board[0][0] === board[2][2]
    ) {
      winner = board[0][0];
      return;
    }
    if (
      board[0][2] &&
      board[0][2] === board[1][1] &&
      board[0][2] === board[2][0]
    ) {
      winner = board[0][2];
      return;
    }
    // Tasuri
    if (board.flat().every((cell) => cell !== null)) {
      winner = 'Draw';
      return;
    }
  };

  // Tämä palauttaa pelialustan tyhjäksi käytin apuna tekoälyä, koska en itse saanut millään korjattua sovellusta niin, että peliruudussa näkyisi voitokas siirto ennen, kuin pop up ilmoitus tulee näkyviin
  const resetBoard = () => {
    board = [
      [null, null, null],
      [null, null, null],
      [null, null, null],
    ];
    currentPlayer = 'X';
    winner = null;
  };
</script>

<main>
  <h1
    style="font-family: 'Pacifico', cursive; font-size: 72px; color: #333; text-align: center; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 40px;"
  >
    Tic Tac Toe
  </h1>
  {#each board as row, rowIndex (rowIndex)}
    <div class="row">
      {#each row as cell, colIndex (colIndex)}
        <div
          class="cell"
          style="background-color: {cell === 'X'
            ? 'red'
            : cell === 'O'
              ? 'black'
              : 'white'}"
          on:click={() => makeMove(rowIndex, colIndex)}
        >
          <span style="font-family: 'Pacifico', cursive; font-size: 72px;"
            >{cell || ''}</span
          >
        </div>
      {/each}
    </div>
  {/each}
  {#if winner !== null}
    <div class="overlay">
      <div class="popup">
        {#if winner === 'Draw'}
          <p>It's a Draw!</p>
        {:else}
          <p>{winner} Wins!</p>
        {/if}
        <button on:click={resetBoard}>Play Again</button>
      </div>
    </div>
  {/if}
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .row {
    display: flex;
  }

  .cell {
    width: 150px;
    height: 150px;
    border: 1px solid black;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
  }

  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .popup {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    text-align: center;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  .popup p {
    font-family: 'Pacifico', cursive;
    font-size: 72px;
    color: #333;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 40px;
    font-size: 24px;
    margin-bottom: 20px;
  }

  .popup button {
    background-color: #4caf50;
    border: none;
    color: white;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    border-radius: 5px;
    cursor: pointer;
  }

  .popup button:hover {
    background-color: #45a049;
  }
</style>
