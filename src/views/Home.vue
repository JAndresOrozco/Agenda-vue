<template>
  <div class="mt-10 sm:mt-0">
  <div class="md:grid md:grid-cols-3 md:gap-6">
    <div class="md:col-span-1">
      <div class="px-4 sm:px-0">
        <h3 class="text-lg font-medium leading-6 text-gray-900">Agenda de Contactos</h3>
      </div>
    </div>
    <div class="mt-5 md:mt-0 md:col-span-2">
      <form>
        <div class="shadow overflow-hidden sm:rounded-md">
          <div class="px-4 py-5 bg-white sm:p-6">
            <div class="grid grid-cols-6 gap-6">
              <div class="col-span-6 sm:col-span-3">
                <label for="name" class="block text-sm font-medium text-gray-700">Nombre</label>
                <input type="text" name="name" id="name" autocomplete="given-name" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" v-model="contact.name">
              </div>

              <div class="col-span-6 sm:col-span-3">
                <label for="last_name" class="block text-sm font-medium text-gray-700">Apellidos</label>
                <input type="text" name="last_name" id="last_name" autocomplete="family-name" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" v-model="contact.last_name">
              </div>

              <div class="col-span-6 sm:col-span-4">
                <label for="email_address" class="block text-sm font-medium text-gray-700">Fecha de Nacimiento</label>
                <input type="date" name="email_address" id="email_address" autocomplete="email" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" v-model="contact.birthday">
              </div>

              <div class="col-span-6 sm:col-span-3 lg:col-span-2">
                <label for="postal_code" class="block text-sm font-medium text-gray-700">Teléfono</label>
                <input type="tel" name="phone" id="phone" autocomplete="phone" class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" v-model="contact.phone">
              </div>
            </div>
          </div>
          <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
            <button class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500" @click="addContact($event)">
              Guardar
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
  <div class="flex flex-col">
  <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
    <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
      <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
        <table class="min-w-full divide-y divide-gray-200">
          <thead class="bg-gray-50">
            <tr>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Nombre
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Apellidos
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Fecha de Nacimiento
              </th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                Teléfono
              </th>
              <th scope="col" class="relative px-6 py-3">
                <span class="sr-only">Edit</span>
              </th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200" >
            <tr v-for="(contact, index) in schedule" :key="contact.id">
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="flex items-center">
                  <div class="ml-4">
                    <div class="text-sm font-medium text-gray-900">
                      {{ contact.name }}
                    </div>
                  </div>
                </div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="text-sm text-gray-900">{{ contact.last_name }}</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="text-sm text-gray-900">{{ contact.birthday }}</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <div class="text-sm text-gray-900">{{ contact.phone }}</div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                <a class="text-indigo-600 hover:text-indigo-900" v-on:click="deleteContact(index)">Eliminar</a>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                <a class="text-indigo-600 hover:text-indigo-900" v-on:click="editContact(index)">Editar</a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<script>

export default {
  data () {
    return {
      contact: { id: '', name: '', last_name: '', birthday: '', phone: '' },
      schedule: [],
      count: 0,
      isEdit: false
    }
  },

  mounted: function () {
    this.loadSchedule()
  },

  methods: {
    loadSchedule: function () {
      var dataSchedule = []
      this.schedule = dataSchedule
      this.count = dataSchedule.length
    },

    addContact: function (e) {
      e.preventDefault()
      if (this.isEdit) {
        if (this.validator) {
          this.schedule[this.contact.index] = { ...this.contact }
          this.isEdit = false
          this.contact = { name: '', last_name: '', birthday: '', phone: '' }
          this.$swal({
            title: 'Éxitoso',
            text: 'Contacto editado con éxito',
            type: 'success',
            showCancelButton: false,
            showCloseButton: true,
            showLoaderOnConfirm: false
          })
        }
      } else {
        if (this.validator) {
          this.count++
          this.contact.id = this.count
          this.schedule.push(this.contact)
          this.contact = { name: '', last_name: '', birthday: '', phone: '' }
          this.$swal({
            title: 'Éxitoso',
            text: 'Contacto agregado con éxito',
            type: 'success',
            showCancelButton: false,
            showCloseButton: true,
            showLoaderOnConfirm: false
          })
        }
      }
    },

    editContact: function (index) {
      this.isEdit = true
      this.contact = { ...this.schedule[index], index }
    },

    deleteContact: function (index) {
      this.$swal({
        title: '¿Estás seguro de que quieres borrar este contacto?',
        text: 'No hay marcha atrás',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Aceptar',
        cancelButtonText: 'Cancelar',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          this.schedule.splice(index, 1)
          this.$swal('Eliminado', 'Eliminación éxitosa', 'success')
        }
      })
    }
  },

  computed: {
    validator () {
      return this.contact.name && this.contact.last_name && this.contact.birthday && this.contact.phone
    }
  }
}
</script>

<style lang="scss" scoped>
  input {
    border-width: 1px;
    padding: .5rem .75rem;
  }
</style>
