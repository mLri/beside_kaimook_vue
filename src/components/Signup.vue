<template>
  <div class="grid-container">
    <Spinner v-if="showSpinner" />
    <div v-if="showContent" class="col-left">
      <div class="logo">
        <img src="../assets/logo.png" alt="">
      </div>
      <div class="welcome">
        <h1>Welcome!</h1>
        <p>
          There are <b>Beside</b>. We are <b>Beside</b> kaimook. <br />
          Of course, you too.
          We are so <b>Strong</b> cuz Baby penguin is so <b>Strong</b>.
          <!-- There are many variations of passages of Lorem Ipsum available, but the majority have suffered alteration in some form, by injected humour -->
        </p>
        <h2>Thank you, you are a big fan.</h2>
      </div>
      
    </div>
    <div v-if="showContent" class="col-right">
      <h3>Sign up</h3>
      <div v-if="showNumberCardInput" class="input-numberCard">
        <div>
          <label class="label-input">number card</label>
        </div>
        <div>
          <input v-model="profile.numberCard" placeholder="0001" type="text" class="input">
        </div>
      </div>
      <div class="input-name">
        <div>
          <label class="label-input">name</label>
        </div>
        <div>
          <span class="error" v-if="errors.firstName">{{ errors.firstName }}</span>
          <input v-model="profile.firstName" @keypress="clearErrorFirstName()" placeholder="John" type="text" class="input">
        </div>
      </div>
      <div class="input-surname">
        <div>
          <label class="label-input">surname</label>
        </div>
        <div>
          <span class="error" v-if="errors.lastName">{{ errors.lastName }}</span>
          <input v-model="profile.lastName" @keypress="clearErrorLastName()" placeholder="Doe" type="text" class="input">
        </div>
      </div>
      <div class="input-email">
        <div>
          <label class="label-input">email</label>
        </div>
        <div>
          <span class="error" v-if="errors.email">{{ errors.email }}</span>
          <input v-model="profile.email" @keypress="clearErrorEmail" placeholder="john@gmail.com" type="text" class="input">
        </div>
      </div>
      <div class="input-tel">
        <div>
          <label class="label-input">telephone</label>
        </div>
        <div>
          <input v-model="profile.tel" type="number" placeholder="0898888888" class="input">
        </div>
      </div>
      <div class="input-sex">
        <div>
          <label class="label-input">sex</label>
        </div>
        <div>
          <input v-model="profile.sex" type="radio" value="m">Men
          <input v-model="profile.sex" type="radio" value="f">Female
          <input v-model="profile.sex" type="radio" value="o">Other
        </div>
      </div>
      <div class="input-age">
        <div>
          <label class="label-input">age</label>
        </div>
        <div>
          <input v-model="profile.age" placeholder="18" type="number" class="input">
        </div>
      </div>
      <div class="input-address">
        <div>
          <label class="label-input">address</label>
        </div>
        <div>
          <span class="error" v-if="errors.addr">{{ errors.addr }}</span>
          <textarea @keypress="clearErrorAddr()" placeholder="55/5 Suphanburi ..." class="input" v-model="profile.addr" rows="4" cols="50">Enter text here...</textarea>
        </div>
      </div>
      <div>
        <button class="save" @click="addMember">Join to baby penguin family</button>
      </div>
    </div>

    <!-- app -->
    <!-- <button id="show-modal" @click="showModal = true">Show Modal</button> -->
    <!-- use the modal component, pass in the prop -->
    <modal v-if="showModal">
        <!--
        you can use custom content here to overwrite
        default content
        -->
        <h3 slot="header">NumberCard</h3>
        <h1 slot="body">{{ profile.numberCard }}</h1>
        <div slot="footer">
          <h4>
            <p>
              Thank you for support Kaimook BNK48
            </p>
            <p>
              you are baby penguin {{ profile.numberCard }} 🐧 ❤️
            </p>
          </h4>
        </div>
    </modal>

  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios'
import Spinner from './Spinner'
import Modal from './Modal'

export default {
  name: "signup",
  components: {
    Spinner,
    Modal
  },
  data() {
    return {
      profile: {
        numberCard: '',
        firstName: '',
        lastName: '',
        email: '',
        tel: '',
        sex: '',
        age: '',
        addr: ''
      },
      showContent: true,
      showSpinner: false,
      showModal: false,
      showNumberCardInput: true,
      errors: {
        firstName: '',
        lastName: '',
        email: '',
        addr: ''
      }
    };
  },
  created() {
    let _this = this;
    document.addEventListener('keyup', function (evt) {
      if (evt.keyCode === 27) {
        _this.closeModal()
      }
    });
  },
  methods: {
    checkData() {
      if (
        this.profile.firstName && 
        this.profile.lastName &&
        this.profile.email &&
        this.profile.addr
      ) {
        return true;
      }

      if (!this.profile.firstName) {
        this.errors.firstName = 'firstName required.'
      }
      if (!this.profile.lastName) {
        this.errors.lastName = 'lastName required.'
      }
      if (!this.profile.email) {
        this.errors.email = 'email required.'
      }
      if (!this.profile.addr) {
        this.errors.addr = 'address required.'
      }
    },
    async addMember() {
      if (await this.checkData()) {
        this.showContent = false
        this.showSpinner = true
        const _this = this
        await axios
          .post("http://localhost:3100/api/member/add", { ...this.profile })
          .then(function(respone) {
            console.log("respone data =>", respone.data);
            // _this.goto("MemberShow");
            if (respone) {
              setTimeout(function(){
                console.log(respone.data)
                _this.showSpinner = false
                _this.showContent = true
                console.log(respone.data.numberCard)
                _this.profile.numberCard = respone.data.data.numberCard
                _this.showModal = true
              }, 1000)
            }
          })
          .catch(function(err) {
            console.log("FAILURE!!", err);
          });
      }
    },
    closeModal() {
      console.log('close modalllllllllllll')
      this.profile.numberCard = ''
      this.profile.firstName = ''
      this.profile.lastName = ''
      this.profile.email = ''
      this.profile.tel = ''
      this.profile.sex = ''
      this.profile.age = ''
      this.profile.addr = ''
      this.showModal = false
    },
    clearErrorFirstName() {
      this.errors.firstName = ''
    },
    clearErrorLastName() {
      this.errors.lastName = ''
    },
    clearErrorEmail() {
      this.errors.email = ''
    },
    clearErrorAddr() {
      this.errors.addr = ''
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.grid-container {
  display: grid;
  grid-template-columns: 50% 50%;
  /* grid-template-rows: 1fr; */
  width: 60%;
  min-height: 500px;
  margin: 0 auto;
}
.col-left {
  display: grid;
  grid-template-rows: 1fr 0.5fr;
  /* align-items: center; */
  /* background-color: #e4f3ea; */
  background-color: #0b86d5;
  width: 100%;
  height: 100%;
  border-radius: 3% 0 0 3%;
}
.col-left img {
  width: 100%;
  background-position: center;
  background-size: cover;
}
.logo {
  display: grid;
  align-items: center;
  margin: 0 auto;
  width: 260px;
}
.col-right {
  display: grid;
  width: 100%;
  height: 100%;
  background-color: #fff;
  color: #b0b0b0;
  border-radius: 0 3% 3% 0;
  padding: 0 10% 0 10%;
}
.col-right div, input, label, textarea {
  /* background-color: transparent; */
  box-sizing: border-box;
  outline: none;
}
.label-input {
  display: block;
  width: 100%;
  text-align: left;
}
.input {
  width: 100%;
  border: none;
  border-bottom: 1px solid #0b86d5;
}
.welcome {
  color: #ffffff;
  text-align: left;
  padding: 20px;
}
.error {
  color: red;
  font-size: 12px;
}
.save {
  background-color: #0b86d575;
  border: none;
  padding: 15px 25px 15px 25px;
  border-radius: 30px;
  color: #FFFFFF;
  font-size: 16px;
  outline: none;
}
.save:hover {
  background-color: #0b86d5;
  cursor: pointer;
}
input::placeholder, textarea::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: #b0b0b0;
  opacity: 0.3; /* Firefox */
}
</style>
