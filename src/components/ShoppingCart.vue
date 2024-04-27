<template>
  <v-container>
    <v-row justify="end">
      <v-col cols="12" md="6" offset-md="3">
        <v-card>
          <v-card-title>Your Cart</v-card-title>
          <v-divider></v-divider>
          <v-list dense>
            <v-list-item-group>
              <v-list-item 
                v-for="(item, index) in cart" 
                :key="index" 
                class="d-flex justify-space-between align-center"
              >
              <div class="d-flex flex-grow-1">
                <v-list-item-content class="d-flex justify-start align-center">
                  <v-list-item-title>{{ item.title }}</v-list-item-title>
                  <v-list-item-subtitle class="ml-4 mr-2">${{ item.price }}</v-list-item-subtitle>
                  <v-btn icon small @click="emitRemoveFromCart(index)">
                    <v-icon small class="small-delete-btn">mdi-delete</v-icon>
                  </v-btn>
                </v-list-item-content>
              </div>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: ['cart'],
  methods: {
    emitRemoveFromCart(index) {
      this.$emit('remove-from-cart', index);
    }
  }
}
</script>

<style scoped>
.small-delete-btn {
  font-size: 16px;
  line-height: 1;
}
</style>