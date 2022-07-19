<template>
  <div class="row justify-center">
    <div class="col-4" v-show="!model">
      <q-select v-model="model" :options="options" label="Выберите репозиторий" @update:model-value="selectStorage" />
    </div>
  </div>
  <div class="row justify-center" v-show="model">
    <div class="col-4">
      <q-input v-model="text" label="Введите имя" />
    </div>
    <div class="col-12">
      <div class="button-add">
        <q-btn color="secondary" :label="'Добавить'" @click="addItem" v-if="!modeEdit" />
      </div>
    </div>
  </div>
  <div class="row justify-center">
    <div class="col-4">
      <q-input square outlined v-model="textSearch" class="search-field" label="Поиск" v-if="storage.length > 0 && !modeEdit" />
    </div>
  </div>
  <div class="row justify-center">
      <div class="col-4">
        <ul>
          <li v-for="(value, key) in listStorage" :key="key">
            <span>{{ value.name }}</span>
            <div class="option-item">
              <q-icon class="option" name="mode_edit" size="1.3em" style="color: #0099e5" @click="editItem(value.name)" v-if="!value.modeEditItem" />
              <q-icon class="option" name="delete" size="1.3em" style="color: #ff5454" @click="deleteItem(value.name)" v-if="!value.modeEditItem" />
              <q-icon class="option" name="done" size="1.3em" style="color: #34bf49" @click="doneEdit(value.name)" v-if="value.modeEditItem" />
            </div>
          </li>
        </ul>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'UsersStorage',
  data() {
    return {
      model: ref(null),
      options: ['Память', 'localStorage'],
      storage: [],
      text: '',
      modeEdit: false,
      textSearch: ''
    }
  },
  methods: {
    selectStorage(model) {
      if (model === 'localStorage') {
        localStorage.setItem('list', JSON.stringify([]));
      }
    },
    addItem() {
      if (this.text.length > 0) {
        if (this.model === 'Память') {
          this.storage.push({
            name: this.text,
            modeEditItem: false
          });
        }

        if (this.model === 'localStorage') {
          let storageList = JSON.parse(localStorage.getItem('list'));

          storageList.push({
            name: this.text,
            modeEditItem: false
          });

          localStorage.setItem('list', JSON.stringify(storageList));
        }

        this.text = '';
      }
    },
    deleteItem(text) {
      if (this.model === 'Память') {
        for (const key in this.storage) {
          if (this.storage[key].name === text) {
            this.storage[key].modeEditItem = true;
          }
        }
      }

      if (this.model === 'localStorage') {
        let storageList = JSON.parse(localStorage.getItem('list'));

        for (const key in storageList) {
          if (storageList[key].name === text) {
            storageList.splice(key, 1);
          }
        }

        this.storage = storageList;

        localStorage.setItem('list', JSON.stringify(storageList));
      }
    },
    editItem(text) {
      if (this.model === 'Память') {
        for (const key in this.storage) {
          if (this.storage[key].name === text) {
            this.storage[key].modeEditItem = true;
          }
        }
      }

      if (this.model === 'localStorage') {
        let storageList = JSON.parse(localStorage.getItem('list'));

        for (const key in storageList) {
          if (storageList[key].name === text) {
            storageList[key].modeEditItem = true;
          }
        }

        this.storage = storageList;

        localStorage.setItem('list', JSON.stringify(storageList));
      }

      this.modeEdit = true;
      this.text = text;
    },
    doneEdit(text) {
      if (this.model === 'Память') {
        for (const key in this.storage) {
          if (this.storage[key].name === text) {
            this.storage[key].modeEditItem = false;
            this.storage[key].name = this.text;
          }
        }
      }

      if (this.model === 'localStorage') {
        let storageList = JSON.parse(localStorage.getItem('list'));

        for (const key in storageList) {
          if (storageList[key].name === text) {
            storageList[key].modeEditItem = false;
            storageList[key].name = this.text;
          }
        }

        this.storage = storageList;

        localStorage.setItem('list', JSON.stringify(storageList));
      }

      this.modeEdit = false;
      this.text = '';
    }
  },
  computed: {
    listStorage() {
      return this.textSearch !== '' ? this.storage.filter(item => item.name.toUpperCase() === this.textSearch.toUpperCase()) : this.storage;
    }
  },
  mounted() {
    localStorage.removeItem('list');
  },
  beforeUpdate() {
    if (this.model === 'localStorage') {
      this.storage = JSON.parse(localStorage.getItem('list'));
    }
  }
}
</script>

<style scoped>
.button-add {
  margin-top: 15px;
}

ul {
  padding-left: 0;
  margin-top: 30px;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-top: 10px;
  list-style-type: none;
  background: #fff5f5;
}

li:first-child {
  margin-top: 0;
}

.option {
  cursor: pointer;
  padding-left: 5px;
}

.search-field {
  margin-top: 20px;
}
</style>
