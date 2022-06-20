<template>
  <div id="modal-window">
    <div class="header-modal">
      <p>Добавить пользователя</p>
      <div style="cursor: pointer;font-size: 20px;" @click="$emit('close')">&times;</div>
    </div>
    <div class="body-modal">
      <div>
        <label for="">Имя</label>
        <input v-model="name" class="newUserName" type="text">
      </div>
      <div>
        <label for="">Номер</label>
        <input v-model="phone" class="newUserPhone" type="text">
      </div>
      <div>
        <label for="">Начальник</label>
        <select name="" id="" class="bossName" v-model="boss">
          <option v-for='item in dataLocalStorage' v-bind:key='item.name'>
            {{item.name}}
          </option>
        </select>
      </div>
      <button v-on:click="addDataInLocalStorage()">Сохранить</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      name: null,
      phone: null,
      boss: null,
      id: 0,
    }
  },
  props: ['dataLocalStorage'],
  mounted () {
  },
  methods: {
    addDataInLocalStorage () {
      this.id = localStorage.length - 1
      if (this.name.trim() && this.phone.trim()) {
        const dataFormAddUser = {
          id: this.id,
          name: this.name,
          phone: this.phone,
          boss: this.boss,
          employees: []
        }
      this.$emit('add-user', dataFormAddUser)
      localStorage.setItem(this.id, JSON.stringify(dataFormAddUser))
      this.id++
      }
    }
  }
}
</script>

<style scoped>
  #modal-window {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    border: 1px solid black;
    padding: 10px;
  }
  .header-modal {
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    align-items: center;
    width: 100%;
  }
  .body-modal > div {
    display: flex;
    justify-content: space-between;
    flex-direction: row;
    align-items: center;
    padding: 10px;
  }
</style>
