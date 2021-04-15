<template>
  <router-view/>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap');
@import './components/styles/checkbox.scss';

* {
  padding: 0;
  margin: 0;
  font-family: 'Poppins', sans-serif;
  box-sizing: border-box;
}

body {
  background-color: #15151C;
}

.input {
  padding: 10px;
  background-color: #E2E3EC;
  color: black;
  border: none;
  border-radius: 5px;
  width: 72%;
  margin-top: 5px;
}

.btn {
  background-color: #15151C;
  padding: 10px 0;
  width: 25%;
  color: white;
  border-radius: 5px;
  border: none;
  outline: none;
  cursor: pointer;
  margin-left: 8px;

  transition: background-color 0.5s;

  &:focus, &:hover {
    background-color: #232330;
  }
}
</style>
