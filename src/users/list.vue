<template>
  <div>
    <Hero
      title="Notendur"
      subtitle="Þeir sem hafa aðgang að gögnunum á einn eða annan hátt."
    />
    <section class="box">
      <table class="table is-fullwidth">
        <thead>
          <tr>
            <th
              class="has-text-centered has-text-grey-light"
              width="9%"
            >
              #
            </th>
            <th width="40%">
              Nafn
            </th>
            <th width="60%">
              Réttindi
            </th>
            <th
              width="1%"
              class="has-text-right"
            >
              <button
                class="button is-small is-info"
                @click="add()"
              >
                <span class="icon">
                  <i class="fas fa-plus" />
                </span>
              </button>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="user in users"
            :key="user.id"
          >
            <td class="has-text-centered has-text-grey-light">{{ user.id }}</td>
            <td>{{ user.name }}</td>
            <td>{{ userType(user) }}</td>
            <td class="has-text-right">
              <button
                class="button is-small is-warning"
                @click="edit(user.id)"
              >
                <span class="icon">
                  <i class="fas fa-pen" />
                </span>
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
import Hero from '../_components/hero'
import makeAPI from '../api'

export default {
  name: 'UsersList',
  components: {
    Hero
  },
  data () {
    return {
      users: []
    }
  },
  created () {
    const usersApi = makeAPI('users')

    usersApi.getAll().then(users => {
      this.users = users
    })
  },
  methods: {
    userType (user) {
      if (user.isAdmin) {
        return 'Stjórnandi'
      }

      if (user.isOrganization) {
        return 'Samtök'
      }

      if (user.isPlace) {
        return 'Staður'
      }
    },
    add () {
      this.$router.push({
        name: 'EditUsers',
        params: { id: 'add' }
      })
    },
    edit (id) {
      this.$router.push({
        name: 'EditUsers',
        params: { id }
      })
    }
  }
}
</script>
