<template>
  <b-container>
    <b-row align-h="center">
      <b-col col lg="6" md="6" sm="4">
        <b-card
          class="custom-card w-100 mt-5"
          header-tag="header"
          header-class="custom-header dfaisb bb-1"
          body-class="p-3"
        >
          <template #header>
            <h3>Contact List</h3>
          </template>
          <div v-for="(contact, index) in contactList" :key="index">
            <b-card class="custom-card w-100 mt-1" body-class="custom-body p-0">
              <b-row align-v="center">
                <b-col cols="3" cols-lg="3"
                  ><div class="p-lg-3">
                    <b-img
                      v-bind="mainProps"
                      rounded="circle"
                      :src="contact.avatar"
                      alt="Circle image"
                    ></b-img></div
                ></b-col>
                <b-col cols="6" cols-lg="7"
                  ><h5>
                    {{ contact.first_name }} {{ contact.last_name }}
                  </h5></b-col
                >
                <b-col cols="2" cols-lg="1">
                  <button
                    type="button"
                    class="btn btn-link"
                    @click="deleteContact(contact.id)"
                  >
                    <b-icon-trash-fill
                      class="h4 mt-2"
                      variant="danger"
                    /></button
                ></b-col>
              </b-row>
            </b-card>
          </div>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "ContactList",
  beforeMount() {
    this.$store.dispatch("getContactList");
  },
  data() {
    return {
      mainProps: {
        width: 65,
        height: 65,
        class: "m1",
      },
    };
  },
  computed: {
    contactList: function () {
      return this.$store.getters.contactList;
    },
  },
  methods:{
    deleteContact: function(id){
      console.log(id)
      this.$store.dispatch("deleteContact", id).then((response) => {
        if(response.status === 204){
          this.$toasted.success("Contact deleted successfully", {
            theme: "bubble",
            position: "top-right",
            duration: 4000
          });
        }
      })
    }
  }
};
</script>

<style>
.custom-card {
  border: none;
  background-color: #fff;
  border-radius: 0.625rem;
  box-shadow: 0 2px 0 rgba(90, 97, 105, 0.11), 0 4px 8px rgba(90, 97, 105, 0.12),
    0 10px 10px rgba(90, 97, 105, 0.06), 0 7px 70px rgba(90, 97, 105, 0.1);
}

.custom-card .custom-header {
  box-shadow: none;
  padding: 1rem 1rem;
  border-radius: 0.625rem 0.625rem 0 0;
  background-color: white;
}

.dfaisb {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.bb-1 {
  border-bottom: 1px solid #e1e5eb;
}

.custom-card > .custom-body {
  border-radius: 0.625rem 0.625rem 0 0;
}

.custom-card .custom-header + .custom-body {
  border-radius: 0 0;
}

</style>
