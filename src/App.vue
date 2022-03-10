<template>
  <div 
    id="app" 
    class="container my-3"
  >
    <header>
      <AppHeaderSearch 
        v-model="userFilter"
        @submit="startFilterCardsByUser"
      />
    </header>

    <main >
      <template v-if="Object.keys(cards).length && isLoadComplite">
        <AppCard
          v-for="(card, cardId) of cards"
          :key="`id_${ cardId }`"
          :title="card.title"
          :body="card.body"
          :userName="users[card.userId].name"
          @clickToUserName="(userFilter = $event) && startFilterCardsByUser()"
        />
      </template>

      <AppLoader 
        v-else-if="isLoadComplite"
        icon="cloud-slash-fill"
        text="Данных нет"
      />

      <AppLoader 
        v-else
        icon="hypnotize"
        text="Загрузка"
        rotate
      />
    </main>
  </div>
</template>

<script>
import AppCard from './components/AppCard.vue';
import AppLoader from './components/AppLoader.vue';
import AppHeaderSearch from './components/AppHeaderSearch.vue';

export default {
  name: 'App',

  data: () => ({
    cards: {},
    users: {},
    userFilter: null,
    isLoadComplite: false,
  }),

  components: {
    AppCard,
    AppLoader,
    AppHeaderSearch,
  },

  methods: {
    async fetchCards(userId) {
      try {
        this.isLoadComplite = false;
        let link = `https://jsonplaceholder.typicode.com/posts`;
        if(userId) link += `?userId=${ userId }`;
        const response = await fetch(link);
        const cards = await response.json();

        const usersPromises = cards.map(card => this.getUserByUserId(card.userId));
        await Promise.all(usersPromises);

        this.cards = cards.reduce((acc, card) => Object.assign(acc, { [card.id]: card }), {});
      } catch(err) {}
      this.isLoadComplite = true;
    },

    startFilterCardsByUser() {
      this.cards = {};
      if(this.userFilter) {
        const user = Object.values(this.users).find(user => user.name.startsWith(this.userFilter));
        if(!user) return;
        this.fetchCards(user.id);
      } else {
        this.fetchCards();
      }
    },

    getUserByUserId(userId) {
      return new Promise(async resolve => {
        try { 
          if(!this.users[userId]) {
            this.users[userId] = {};
            const response = await fetch(`https://jsonplaceholder.typicode.com/users/${ userId }`);
            this.users[userId] = await response.json();
          }
          resolve();
        } catch(err) {}
      })
    },
  },

  async beforeMount() {
    this.fetchCards();
  },
};
</script>

<style lang="scss">
:root {
  --app-body-color: #ECF1F6;
  --app-loader-color: #CED4DA;
}

body {
  background-color: var(--app-body-color) !important;

  header {
    form {
      display: grid;
      grid-auto-columns: minmax(350px, calc((100% - 2rem)/3));
      gap: 1rem;
      justify-content: center;
    }
  }

  main {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    gap: 1rem;
  }
}

@media screen and (max-width: 768px) {
  body {
    header form, main {
      grid-template-columns: repeat(1, 1fr);
    }
  }
}
</style>
