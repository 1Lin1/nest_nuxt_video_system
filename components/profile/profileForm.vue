<template>
  <v-card class="overflow-hidden mt-1" color="dark lighten-1" dark>
    <v-toolbar flat color="purple">
      <v-icon>mdi-account</v-icon>
      <v-toolbar-title class="font-weight-light">个人信息</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn color="purple darken-3" fab small @click="isEditing = !isEditing">
        <v-icon v-if="isEditing">mdi-close</v-icon>
        <v-icon v-else>mdi-pencil</v-icon>
      </v-btn>
    </v-toolbar>

    <v-card-text>
     <v-form
       ref="form"
       v-model="valid"
       :lazy-validation="lazy"
     >

      <v-text-field
        :disabled="!isEditing"
        :rules="rules.nameRules"
        color="white"
        label="姓名"
        required
        v-model="profileFormModel.profileName"
      ></v-text-field>
      <v-autocomplete
        :rules="rules.sexRules"
        :disabled="!isEditing"
        :items="states"
        :filter="customFilter"
        color="white"
        item-text="name"
        label="性别"
        v-model="profileFormModel.profileSex"
        required
      ></v-autocomplete>

      <v-text-field
        :rules="rules.ageRules"
        :disabled="!isEditing"
        color="white"
        label="年龄"
        v-model="profileFormModel.profileAge"
        required
      ></v-text-field>

     </v-form>

       <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn :disabled="!valid" color="success" @click="save">
        Save
      </v-btn>
    </v-card-actions>
    </v-card-text>
    <v-divider></v-divider>
  
    <v-snackbar v-model="hasSaved" :timeout="2000" absolute bottom left>
      个人信息 已修改
    </v-snackbar>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      hasSaved: false,
      isEditing: null,
      profileFormModel: {},
      states: [
        { name: '男', abbr: 'FL', id: 1 },
        { name: '女', abbr: 'GA', id: 2 }
      ],
      valid: true,
      lazy: false,
      //输入规则
      rules:{
        nameRules: [
        v => !!v || '姓名不能为空',
        v => (v && v.length <= 10) || '姓名必须少于10个字',
      ],

      sexRules:[
            v => !!v || '性别不能为空',
            v => v == '男' || v == '女' || '请在性别男女中 至少选择一项'
      ],

      ageRules:[
           v => !!v || '年龄不能为空',
            v => (v && v <= 140) || '年龄不能大于140',
      ]

      }
    }
  },

  methods: {

    //验证输入规则
     validate () {
        this.$refs.form.validate()
      },


    customFilter(item, queryText, itemText) {
      const textOne = item.name.toLowerCase()
      const textTwo = item.abbr.toLowerCase()
      const searchText = queryText.toLowerCase()

      return (
        textOne.indexOf(searchText) > -1 || textTwo.indexOf(searchText) > -1
      )
    },
    async save() {
      //test
      const res = await this.$axios.$get('auth/user')
      console.log(res._id)
      //修改个人信息
      await this.$axios.$put(`users/${res._id}`, this.profileFormModel)

      // 修改vuex
      this.$store.state.auth.user.profileName = this.profileFormModel.profileName
      this.$store.state.auth.user.profileSex = this.profileFormModel.profileSex
      this.$store.state.auth.user.profileAge = this.profileFormModel.profileAge

      this.isEditing = !this.isEditing
      this.valid = true
      console.log(this.profileFormModel)
      this.hasSaved = true
    }
  },

  computed: {
    profileFormData() {
      const res = {
        profileName: this.$store.state.auth.user.profileName,
        profileSex: this.$store.state.auth.user.profileSex,
        profileAge: this.$store.state.auth.user.profileAge
      }
      return res
    }
  },

  mounted() {
    this.profileFormModel = this.profileFormData;
  }
}
</script>
