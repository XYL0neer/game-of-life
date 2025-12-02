<template>
  <main>
    <p>Implementation availabe at <a href="https://github.com/XYL0neer/game-of-life">GitHub</a></p>
    <form method="get" action="game">
      <label>Width
        <input name="width" value="10">
      </label>
      <label>Height
        <input name="height" value="10">
      </label>
      <label>Initial living cells
        <input name="initialLifePool" value="30">
      </label>
      <button type="submit">Start</button>
    </form>
  </main>
</template>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  gap: .4rem;
  margin-top: 2rem;
}
</style>
