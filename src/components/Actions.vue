<template>
  <div>
    <b-btn size="sm" @click="modalView = true" variant="outline-primary">Visualizar</b-btn>
    <b-btn size="sm" @click="modalEdit = true" variant="outline-primary">Editar</b-btn>
    <b-btn size="sm" @click="modalDel = true" variant="danger">Deletar</b-btn>

    <b-modal size="lg" v-model="modalView" :title="`${user.name.first} ${user.name.last}`" hide-footer>
      <div class="d-flex">
        <div class="d-flex flex-column align-items-center text-center">
          <img :src="user.picture" alt="Admin" class="rounded-circle" width="150">
          <div class="mt-3">
            <h4>{{ `${user.name.first} ${user.name.last}` }}</h4>
            <p class="text-secondary mb-1">Firma: {{ user.company }}</p>
            <button class="btn btn-primary">Seguir</button>
            <button class="btn btn-outline-primary">Mensagem</button>
          </div>
        </div>

        <div class="d-flex flex-column ml-4 flex-grow-1">
          <div class="d-flex border-bottom py-1">
            <h6 class="profile-item m-0">Nome Completo</h6>
            <div class="text-secondary">{{ `${user.name.first} ${user.name.last}` }}</div>
          </div>

          <div class="d-flex border-bottom py-1">
            <h6 class="profile-item m-0">Email</h6>
            <div class="text-secondary">{{ user.email }}</div>
          </div>

          <div class="d-flex border-bottom py-1">
            <h6 class="profile-item m-0">Idade</h6>
            <div class="text-secondary">{{ user.age }}</div>
          </div>

          <div class="d-flex border-bottom py-1">
            <h6 class="profile-item m-0">Telefone</h6>
            <div class="text-secondary">{{ user.phone }}</div>
          </div>

          <div class="d-flex py-1">
            <h6 class="profile-item m-0">Endereço</h6>
            <div class="text-secondary">{{ user.address }}</div>
          </div>
        </div>
      </div>
    </b-modal>  

    <b-modal v-model="modalEdit" ref="modal" :title="`${user.name.first} ${user.name.last}`" hide-footer>
      <b-form @submit="(event) => {$emit('update-user', event, user, form); modalEdit = false}">
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

    <b-modal v-model="modalDel" ref="modal" hide-footer>
      <template #modal-title>
        Deletar <code>{{ user.name.first }} {{ user.name.last }}</code>?
      </template>

      <div class="d-block text-center">
        <h5>Você tem certeza que deseja deletar este usuário?</h5>

        <b-button class="mt-3" variant="danger" @click="$emit('remove-user', user); modalDel = false">Deletar</b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Actions',
  props: ['user'],
  data: () => ({
    modalView: false,
    modalEdit: false,
    modalDel: false,
    form: {},
  }),
  created() {
    // removendo observer do objeto
    this.form = JSON.parse(JSON.stringify(this.user));
  },
};
</script>

<style>
.profile-item {
  width: 25%;
}
</style>