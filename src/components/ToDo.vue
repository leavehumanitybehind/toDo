<template>
  <div
    class="main-wrapper"
    v-bind:class="[{ lightWrapper: light, darkWrapper: !light }]"
  >
    <div class="header">
      <div class="header__text">
        <h1 class="text-body-title">TO DO</h1>
        <button
          class="header__btn"
          v-bind:class="[{ lightBtn: light, darkBtn: !light }]"
          @click="light = !light"
        ></button>
      </div>
      <div class="header__input-wrapper">
        <input
          type="checkbox"
          name="header-checkbox"
          id="header-checkbox"
          class="header__checkbox"
          v-model="checked"
        />
        <label for="header-checkbox" class="header__label"> </label>
        <input
          class="header__input"
          type="text"
          v-model="newItem"
          @keyup.enter="addToDo(newItem)"
          placeholder="Create a new todo..."
        />
      </div>
    </div>
    <div class="main" v-if="toDos.length">
      <ul class="todo__list">
        <draggable v-model="currentToDos">
          <transition-group>
        <li
          v-for="(todo, index) in currentToDos"
          :key="index"
          class="body-text1 todo__list-item"
        >
          <input
            v-model="todo.completed"
            :id="todo.name + index"
            type="checkbox"
            name="checkbox"
            class="todo__list-checkbox"
          />
          <label :for="todo.name + index" class="todo__list-label">
            {{ todo.name }}
          </label>
          <button
            @click="deleteToDo(todo)"
            class="todo__list-btn"
            aria-hidden="close"
          ></button>
        </li>
        </transition-group>
        </draggable>
        <div class="todo__list-footer">
          <p class="text-body2">{{ checkActiveToDo }} items left</p>
          <a class="text-body2 clear-btn" @click="deleteAllCompleted()">
            Clear completed</a
          >
        </div>
      </ul>
      <ul class="footer">
        <li
          v-bind:class="{ active: links.item1 }"
          @click="currentSorting = 'All'"
        >
          <a class="text-body3" id="item1" v-on:click="toggleActiveLink">All</a>
        </li>
        <li
          v-bind:class="{ active: links.item2 }"
          @click="currentSorting = 'Active'"
        >
          <a class="text-body3" id="item2" v-on:click="toggleActiveLink"
            >Active</a
          >
        </li>
        <li
          v-bind:class="{ active: links.item3 }"
          @click="currentSorting = 'Completed'"
        >
          <a class="text-body3" id="item3" v-on:click="toggleActiveLink"
            >Completed</a
          >
        </li>
      </ul>
      <p class="text-body2 footer__drag">Drag and drop to reorder list</p>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
export default {
  name: "ToDo",
  data() {
    return {
      toDos: [],
      activeToDos: [],
      completedToDos: [],
      light: true,
      newItem: "",
      links: {},
      checked: false,
      currentSorting: "All",
    };
  },
  components: {
draggable
  },
  methods: {
    toggleActiveLink(e) {
      e.preventDefault();
      const id = e.target.id;

      this.links = {};
      this.links[id] = true;
    },
    addToDo(item) {
      let newItem = {};
      if (item.length) {
        if (this.checked) {
          newItem = {
            name: item,
            active: false,
            completed: true,
          };
        } else {
          newItem = {
            name: item,
            active: true,
            completed: false,
          };
        }
        this.toDos.unshift(newItem);
      }
      this.newItem = "";
      this.checked = false;
    },
    deleteToDo(index) {
      this.toDos.splice(index, 1);
    },
    deleteAllCompleted() {
      const completedToDos = this.toDos.filter(
        (item) => item.completed === false
      );
      this.toDos = completedToDos;
      return this.toDos;
    },
    showActiveToDos() {
      const toDos = this.toDos.filter((item) => item.completed === false);
      return (this.completedToDos = toDos);
    },
    showCompletedToDos() {
      const toDos = this.toDos.filter((item) => item.completed === true);
      return (this.completedToDos = toDos);
    },
  },
  computed: {
    checkActiveToDo: function () {
      const activeToDos = this.toDos.filter((item) => item.completed === false);
      return activeToDos.length;
    },
    currentToDos: function () {
      switch (this.currentSorting) {
        case "All":
          return this.toDos;
        case "Active":
          return this.showActiveToDos();
        case "Completed":
          return this.showCompletedToDos();
      }
      console.log(this.currentSorting);
      return this.currentSorting;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
:root {
  --white: #ffffff;
  --lightTextColor: #494c6b;
  --darkTextColor: #c8cbe7;
  --darkBackground: #25273d;
}
.main-wrapper {
  background-repeat: no-repeat;
  background-size: 100% 200px;
  min-height: 600px;
}

@media (min-width: 868px) {
  .main-wrapper {
    background-size: 100% 300px;
  }
}

.lightWrapper {
  color: var(--lightTextColor);
  background-color: #f2f2f2;
  background-image: url("../assets/light-theme.jpg");
}

.darkWrapper {
  color: var(--darkTextColor);
  background-color: #171823;
  background-image: url("../assets/dark-theme.jpg");
}

.header,
.main {
  margin: 0 26px;
}

@media (min-width: 868px) {
  .header,
  .main {
    margin: 0 250px;
  }
}

@media (min-width: 1266px) {
  .header,
  .main {
    margin: 0 450px;
  }
}

.text-body-title {
  font-weight: bold;
  font-size: 32px;
  line-height: 1;
  color: var(--white);
  letter-spacing: 15px;
  margin: 0;
  padding: 0;
}

@media (min-width: 868px) {
  .text-body-title {
    font-size: 40px;
  }
}

.text-body1 {
  line-height: 1;
  font-weight: normal;
  letter-spacing: -0.166667px;
}

@media (min-width: 868px) {
  .text-body1 {
    font-size: 18px;
  }
}

.text-body2 {
  line-height: 1;
  letter-spacing: -0.166667px;
  color: #9495a5;
}

.darkWrapper .text-body2 {
  color: #5b5e7e;
}

@media (min-width: 868px) {
  .text-body2 {
    font-size: 14px;
    letter-spacing: -0.194444px;
  }
}

.text-body3 {
  font-weight: bold;
  font-size: 14px;
  line-height: 1;
  letter-spacing: -0.194444px;
  color: #9495a5;
}

.text-body3:hover,
.clear-btn:hover {
  cursor: pointer;
  color: var(--lightTextColor);
}

.darkWrapper .text-body3 {
  color: #5b5e7e;
}

.darkWrapper .text-body3:hover,
.darkWrapper .clear-btn:hover {
  color: #e3e4f1;
}

.active .text-body3 {
  color: #3a7cfd;
}

.header__text {
  display: flex;
  justify-content: space-between;
  padding-top: 48px;
  margin-bottom: 48px;
}

@media (min-width: 868px) {
  .header__text {
    padding-top: 70px;
    margin-bottom: 32px;
  }
}

.header__btn {
  border: none;
  background-color: transparent;
  background-repeat: no-repeat;
  background-position: center;
  width: 30px;
  height: 30px;
}

.header__btn:hover {
  transform: scale(1.2);
}

.lightBtn {
  background-image: url("../assets/sun-btn.svg");
}

.darkBtn {
  background-image: url("../assets/moon-btn.svg");
}

.header__input {
  border: none;
  caret-color: #3a7cfd;
  outline: none;
  margin: 18px 0 18px 22px;
}

.header__input-wrapper {
  box-sizing: border-box;
  width: 100%;
  display: flex;
  align-items: center;
  background-color: var(--white);
  border-radius: 5px;
  margin-bottom: 16px;
  padding-left: 24px;
}

@media (min-width: 868px) {
  .header__input-wrapper {
    margin-bottom: 24px;
  }
}

.todo__list {
  width: 100%;
  background-color: var(--white);
  border-radius: 5px;
  list-style: none;
  margin: 0;
  padding: 0;
  box-shadow: 0px 4px 4px 0px #00000040;
}

.darkWrapper .todo__list,
.darkWrapper .header__input-wrapper,
.darkWrapper .header__input,
.darkWrapper .footer {
  background-color: var(--darkBackground);
}

.todo__list-item {
  padding: 21px 24px;
  border-bottom: 1px solid #e3e4f1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.darkWrapper .todo__list-item {
  border-color: #393a4b;
}

.todo__list-btn {
  background-image: url("../assets/close.svg");
  background-repeat: no-repeat;
  background-position: center;
  border: none;
  background-color: transparent;
  width: 12px;
  height: 12px;
  cursor: pointer;
}

@media (min-width: 868px) {
  .todo__list-btn {
    width: 18px;
    height: 18px;
    opacity: 0;
  }

  .todo__list-item:hover .todo__list-btn {
    opacity: 1;
  }
}
.todo__list-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
}

/* checkbox style */

.todo__list-checkbox,
.header__checkbox {
  position: absolute;
  z-index: -1;
  opacity: 0;
}

.todo__list-checkbox + label,
.header__checkbox + .header__label {
  display: inline-flex;
  align-items: center;
  user-select: none;
  position: relative;
}

.todo__list-checkbox + label::before,
.header__checkbox + .header__label::before {
  content: "";
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 1px solid #d8d8d8;
  margin-right: 12px;
  border-radius: 50%;
}

.darkWrapper .todo__list-checkbox + label::before {
  border-color: #393a4b;
}

.todo__list-checkbox:checked + label::before,
.header__checkbox:checked + .header__label::before,
.header__checkbox:not(:disabled):not(:checked) + .header__label:hover::before,
.todo__list-checkbox:not(:disabled):not(:checked) + label:hover::before {
  background-image: url("../assets/circle-path.svg");
  background-position: center;
  border: none;
  width: 22px;
  height: 22px;
}

.todo__list-checkbox:checked + label {
  text-decoration: line-through;
  color: #d1d2da;
}

.darkWrapper .todo__list-checkbox:checked + label {
  color: #4d5067;
}

/* стили при наведении курсора на checkbox */
.todo__list-checkbox:not(:disabled):not(:checked) + label:hover::after,
.header__checkbox:not(:disabled):not(:checked) + .header__label:hover::after {
  content: "";
  position: absolute;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--white);
  left: 1px;
}

/* стили для активного состояния чекбокса (при нажатии на него) */
.todo__list-checkbox:not(:disabled):active + label::before {
  background-color: #b3d7ff;
  border-color: #b3d7ff;
}
/* стили для чекбокса, находящегося в фокусе */
.todo__list-checkbox:focus + label::before {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}
/* стили для чекбокса, находящегося в фокусе и не находящегося в состоянии checked */
.todo__list-checkbox:focus:not(:checked) + label::before {
  border-color: #80bdff;
}
/* стили для чекбокса, находящегося в состоянии disabled */
.todo__list-checkbox:disabled + label::before {
  background-color: #e9ecef;
}

.footer {
  display: flex;
  justify-content: center;
  list-style: none;
  background-color: var(--white);
  padding: 15px 0;
  box-shadow: 0px 35px 50px -15px rgba(194, 195, 214, 0.5);
  border-radius: 5px;
  margin-bottom: 40px;
}

@media (min-width: 968px) {
  .footer {
    margin-bottom: 0;
    margin-top: -43px;
    padding: 0;
    box-shadow: none;
  }
}

.footer li:not(:last-child) {
  margin-right: 10px;
}

.footer__drag {
  text-align: center;
  margin-bottom: 52px;
}

@media (min-width: 968px) {
  .footer__drag {
    margin-top: 49px;
  }
}
</style>
