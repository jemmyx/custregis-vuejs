<template>
  <v-layout>
    <v-flex xs12 sm6 offset-sm3>
      <v-card>
		<h1 style="background:#3d3e48;color:white;padding-top:20px;padding-bottom:20px;">{{title}}
		<br/>
		<span style="font-size:17px;font-weight:normal;">N'attendez plus et profitez de l'offre d'automne !</span>
		</h1>
        <v-img
          :src="header"
          aspect-ratio="2.75"
        ></v-img>
        <v-card-title primary-title>


  <v-form ref="form" v-model="valid" lazy-validation style="width:100%;" >
  
    <v-text-field
      v-model="name"
      :rules="nameRules"
      :counter="32"
      label="Nom"
      required
    ></v-text-field>

    <v-text-field
      v-model="firstname"
      :rules="firstnameRules"
      :counter="32"
      label="Prénom"
      required
    ></v-text-field>

    <v-text-field
      v-model="mobile"
      :rules="mobileRules"
      label="N. de mobile (ex. 078 444 33 22 11)"
      required
    ></v-text-field>

	<v-checkbox
	  v-model="checkboxWhatsapp"
	  label="S'inscrire au groupe WhatsApp"
	></v-checkbox>

    <v-text-field
      v-model="email"
      :rules="emailRules"
      label="E-mail"
      required
    ></v-text-field>

	<v-radio-group 
		v-model="radioSex" 
		:mandatory="false"
		required
		:rules="[v => !!v || 'Veuillez indiquer le genre.']"
		>
	  <v-radio label="Homme" value="m"></v-radio>
	  <v-radio label="Femme" value="f"></v-radio>
	</v-radio-group>

    <v-btn
      :disabled="!valid"
      @click="submit"
    >
      Envoyer
    </v-btn>
    <v-btn @click="clear">Effacer</v-btn>
	
	<v-checkbox
      v-model="checkboxAgreed"
      :rules="[v => !!v || 'Vous devez accepter les conditions avant de continuer.']"
      label="J'accepte les conditions"
      required
    ></v-checkbox>

	<v-alert
	  :color="alertColor"
      :value="displayAlert"
      type="success"
    >
	{{alertText}}
    </v-alert>
	
  </v-form>


        </v-card-title>

        <v-card-actions>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>

</template>

<script>
  import axios from 'axios'

  export default {
  
	props:{
		title:String,
		header:String,
		referer:String
	},
    data: () => ({
      valid: true,

      name: '',
      nameRules: [
        v => !!v || 'Le nom est requis',
        v => (v && v.length <= 32) || 'Le nom doit être inférieure à 32 car.'
      ],

      firstname: '',
      firstnameRules: [
        v => !!v || 'Le prénom est requis',
        v => (v && v.length <= 32) || 'Le prénom doit être inférieure à 32 car.'
      ],

      mobile: '',
      mobileRules: [
        v => !!v || 'Le mobile est requis',
        v => (v && v.length <= 16) || 'Le mobile doit être valide'
      ],

      email: '',
      emailRules: [
        v => !!v || 'L\'E-mail est requis',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      
	  radioSex:'',
	  
	  checkboxWhatsapp:true,
	  
	  checkboxAgreed: false,
	  
	  displayAlert: false,
	  alertColor: 'green',
	  alertText: 'Inscription enregistrée avec succès ! Un code promo peronnel a été envoyé sur votre mobile.',
	  
    }),

    methods: {
      submit () {

        if (this.$refs.form.validate()) {

			let whatsapp = this.checkboxWhatsapp == true ? 1 : 0

			axios.get('//core.mayan-payments.com/api/crm/registerUser?firstname='+this.firstname+'&name='+this.name+'&email='+this.email+'&mobile='+this.mobile+'&sex='+this.radioSex+'&whatsapp='+whatsapp+'&referer='+this.referer, {
				name:this.name,
				firstname:this.firstname,
				email:this.email,
				mobile:this.mobile,
				sex:this.radioSex,
				whatsapp:whatsapp,
				referer:this.referer
			})
			.then(response => {
				
				this.displayAlert=false
				
				if(response.data.success){
					this.alertColor = 'green'
					this.alertText = 'Inscription enregistrée avec succès ! Un code promo peronnel a été envoyé sur votre mobile.'
					this.valid=false
				}else{
					this.alertText = response.data.messageerror
					this.alertColor = 'red'
				}
				
				this.displayAlert = true
			})
        }
      },
      clear () {
        this.$refs.form.reset()
      }
    }
  }
</script>