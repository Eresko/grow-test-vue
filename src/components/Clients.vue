<script setup>
import { ref,watchEffect,onMounted,reactive,computed } from 'vue'
import { debouncedWatch } from '@vueuse/core'
import axios from 'axios'
const clients = ref([]);
import { Notyf } from 'notyf'
import 'notyf/notyf.min.css';
const notyf = new Notyf()
const filters = ref({
  current_page: 1,
  per_page: 10,
  total: 0,
  last_page: 0,
})
const searchText = ref("");
debouncedWatch(searchText, () => getClients(), { debounce: 500 })
const page = reactive({current: 1,old:1})
const getClients = async () => {
  await axios.get('https://api-grow-test.zedform.ru/api/clients?page=' + page.current + '&search=' + searchText.value ).then( response  => {
    console.log(response);
    clients.value = response.data.result.data
    filters.value.current_page = response.data.result.currentPage
    filters.value.per_page = response.data.result.perPage
    filters.value.total = response.data.result.total
    filters.value.last_page = response.data.result.lastPage
    notyf.success('update list')
  }).catch( res => {
    notyf.error('error')
  })

}
watchEffect(() => {
  if (page.current !== page.old) {
    page.old = page.current;
    getClients();
  }
})
onMounted( async () => {
  await getClients();
})
</script>


<template>
  <div class="clients">
    <div class="title">Клиенты</div>
    <div class="search">
      <div class="search__block">
        <input type="text" class="search__input" v-model="searchText">
        <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 -960 960 960" width="24"><path d="M784-120 532-372q-30 24-69 38t-83 14q-109 0-184.5-75.5T120-580q0-109 75.5-184.5T380-840q109 0 184.5 75.5T640-580q0 44-14 83t-38 69l252 252-56 56ZM380-400q75 0 127.5-52.5T560-580q0-75-52.5-127.5T380-760q-75 0-127.5 52.5T200-580q0 75 52.5 127.5T380-400Z"/></svg>
      </div>
    </div>
    <div class="clients_length">
      <span>Клиентов:</span>
      <span>{{ clients.length }}</span>
    </div>
    <div class="body_clients">
      <div class="body_clients__header">
        <div class="body_clients__check_box">
          <input type="checkbox" class="body_clients__checkbox">
        </div>
        <div class="body_clients__name">
          Название
        </div>
        <div class="body_clients__phone">
          Телефон
        </div>
        <div class="body_clients__email">
          Email
        </div>
        <div class="body_clients__birthday">
          День рождения
        </div>
      </div>
      <div class="body_clients__line"></div>
      <div class="body_clients__client">
        <div class="body_clients__clients" v-for="client in clients">
          <div class="body_clients__check_box">
            <input type="checkbox" class="body_clients__checkbox">
          </div>
          <div class="body_clients__name">
            {{ client.name }}
          </div>
          <div class="body_clients__phone">
            {{ client.phone }}
          </div>
          <div class="body_clients__email">
            {{  client.email  }}
          </div>
          <div class="body_clients__birthday">
            {{  client.birthday  }}
          </div>
        </div>
        <div class="body_clients__line"></div>
      </div>
    </div>

  </div>



</template>


<style lang="scss" scoped>
.body_clients {
  &__line {
    border-bottom: 1px solid #a5a5a5;
    margin-bottom: 5px;
  }
  &__clients {
    display: flex;
    width: 800px;
    color: #252733;
    font-family: Mulish;
    font-size: 14px;
    font-weight: 400;
    line-height: 10px;
    letter-spacing: 0.20000000298023224px;
    text-align: left;
  }
  &__header {
    display: flex;
    width: 800px;
    color: #252733;
    font-family: Mulish;
    font-size: 14px;
    font-weight: 400;
    line-height: 10px;
    letter-spacing: 0.20000000298023224px;
    text-align: left;
  }
  &__checkbox {
    width: 20px;
    height: 20px;
    background: #ffffff;
    fill: white;
  }
  &__check_box {
    background: white;
    width: 10%;
  }
  &__name {
    display: flex;
    align-items: center;
    width: 20%;
  }
  &__phone {
    display: flex;
    align-items: center;
    width:20%;
  }
  &__email {
    display: flex;
    align-items: center;
    width:20%;
  }
  &__birthday {
    display: flex;
    align-items: center;
    width:20%;
  }

  &__check_box {

  }
}
.clients_length {
  margin-top: 5px;
  margin-bottom: 20px;
  color: #1a1a1a;
  font-family: Mulish;
  font-size: 14px;
  font-weight: 900;
  line-height: 20px;
  letter-spacing: 0.20000000298023224px;
  text-align: left;
}
.search {
  display: flex;
  &__block {
    position: relative;
    border: 1px solid #a5a5a5;
    border-radius: 5px;
    width: 400px;
    height: 30px;
  }
  &__input {
    left:30px;
    height: 27px;
    width: 360px;
    position: absolute;
    color: #1a1a1a;
    background-color: white;
    border: none;
    outline:none;
  }
  svg {
    position: absolute;
    left: 5px;
    height: 28px;
    cursor: pointer;
  }
}
.clients {
  margin: 120px;
  width: 1100px;
}
.title {
  margin-bottom: 20px;
  color: #1a1a1a;
  font-family: Mulish;
  font-size: 14px;
  font-weight: 900;
  line-height: 20px;
  letter-spacing: 0.20000000298023224px;
  text-align: left;

}


</style>