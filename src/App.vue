<template>
  <div id="app">
    <b-navbar type="dark" variant="dark">
      <b-navbar-brand href="#">Operand CRUD - VueJS</b-navbar-brand>
    </b-navbar>

    <b-container fluid="xl">
      <div class="d-flex py-2 justify-content-between">
        <h2>Usuários</h2>

        <b-btn @click="modalCreate = true" variant="success">Adicionar Usuário</b-btn>
      </div>

      <b-table striped hover :items="users" :fields="fields">
        <template #cell(name)="data">
          {{ `${data.value.first} ${data.value.last}` }}
        </template>

        <template #cell(actions)="data">   
          <Actions :user="data.item" @remove-user="removeUser" @update-user="updateUser"/>
        </template>
      </b-table>

      <b-modal v-model="modalCreate" title="Adicionar Usuário" ref="modal" hide-footer>
        <b-form @submit="(event) => createUser(event)">
          <b-form-group id="firstName" label="Nome: " label-for="firstName">
            <b-form-input id="firstName" v-model="form.name.first" type="text" required />
          </b-form-group>

          <b-form-group id="lastName" label="Sobrenome:" label-for="lastName">
            <b-form-input id="lastName" v-model="form.name.last" type="text" required />
          </b-form-group>

          <b-form-group id="email" label="Email:" label-for="email">
            <b-form-input id="email" v-model="form.email" type="email" required />
          </b-form-group>

          <b-form-group id="age" label="Idade:" label-for="age">
            <b-form-input id="age" v-model="form.age" type="number" required />
          </b-form-group>

          <b-form-group id="phone" label="Telefone:" label-for="phone">
            <b-form-input id="phone" v-model="form.phone" type="text" required />
          </b-form-group>

          <b-form-group id="address" label="Endereço:" label-for="address">
            <b-form-input id="address" v-model="form.address" type="text" required />
          </b-form-group>

          <b-button type="submit" variant="primary" class="float-right">Salvar</b-button>
        </b-form>
      </b-modal>
    </b-container>
  </div>
</template>

<script>
import usersBase from './assets/users';
import Actions from './components/Actions';

export default {
  name: 'app',
  components: { Actions },
  data: () => ({
    fields: [
      {
        key: 'id',
        label: '# ID',
      },
      {
        key: 'name',
        label: 'Nome',
        sortable: true,
      },
      {
        key: 'age',
        label: 'Idade',
        sortable: true,
      },
      {
        key: 'email',
        label: 'E-mail',
        sortable: true,
      },
      {
        key: 'actions',
        label: 'Ações',
      },
    ],
    users: usersBase,
    modalCreate: false,
    form: {
      name: {
        first: '',
        last: '',
      },
      picture: 'http://placehold.it/75x75',
    },
  }),
  methods: {
    removeUser(user) {
      const removeIndex = this.users.map(item => item.id).indexOf(user.id);
      this.users.splice(removeIndex, 1);

      this.$bvToast.toast(`O usuário ${user.name.first} foi deletado!`, {
        title: 'Usuário Deletado',
        autoHideDelay: 5000,
        variant: 'success',
        solid: true,
      });
    },
    createUser(e) {
      e.preventDefault();

      this.form.id = this.idGenerator();

      this.users.push(this.form);

      this.$bvToast.toast(`O usuário ${this.form.name.first} foi criado com sucesso!`, {
        title: 'Usuário Criado',
        autoHideDelay: 5000,
        variant: 'success',
        solid: true,
      });

      this.form = {
        name: {
          first: '',
          last: '',
        },
        picture: 'http://placehold.it/75x75',
      };

      this.modalCreate = false;
    },
    idGenerator() {
      const timestamp = (new Date().getTime() / 1000 || 0).toString(16);

      return timestamp + (Math.random() * 16 || 0).toString(16);
    },
    updateUser(e, oldUser, updatedUser) {
      e.preventDefault();

      const userIndex = this.users.map(item => item.id).indexOf(oldUser.id);

      this.$set(this.users, userIndex, updatedUser);

      this.$bvToast.toast('O usuário foi atualizado!', {
        title: 'Usuário Atualizado',
        autoHideDelay: 5000,
        variant: 'success',
        solid: true,
      });
    },
  },
};
</script>

<style>
</style>
