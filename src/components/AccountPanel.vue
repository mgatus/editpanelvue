<template>
  <div class="account-panel">
    <div class="actions">
      <button 
        v-if="!isEditing" 
        @click="isEditing = true" 
        id="edit" 
        title="Edit account details"
        aria-label="Edit account details"
      >
        <i class="fas fa-edit" aria-hidden="true"></i>
      </button>
      <button 
        v-else 
        @click="saveChanges" 
        id="save" 
        title="Save changes"
        aria-label="Save changes"
      >
      <i class="fas fa-save"></i>
      </button>
    </div>
    <div class="avatar-container">
      <img width="512" height="512" :src="gravatarUrl" class="avatar" alt="Profile" />
    </div>

    <div class="field nameContainer">
      <template v-if="isEditing">
        <label>Edit Name</label>
        <input v-model="firstName" placeholder="First Name" :class="{ 'input-error': toast.error && !firstName }" />
        <input v-model="lastName" placeholder="Last Name" :class="{ 'input-error': toast.error && !lastName }" />
      </template>
      <template v-else>
        <h2 class="fullName">{{ firstName }} {{ lastName }} <i class="fa-solid fa-check-circle verified"></i></h2>
      </template>
    </div>

    <div class="field emailContainer">
      <template v-if="isEditing">
        <label>Change Email</label>
        <input v-model="email" placeholder="Email" :class="{ 'input-error': toast.error && !validateEmail(email) }" />
      </template>
      <template v-else>
        <p class="emailAddress"><i class="fa fa-envelope email-icon" aria-hidden="true"></i> {{ email }}</p>
      </template>
    </div>

    <div class="field passwordContainer">
      <div v-if="isEditing" class="toggle">
        <label>Change Password</label>
        <div class="field-group">
          <div class="input-icon-wrapper">
            <input
              :type="showPassword ? 'text' : 'password'"
              v-model="password"
              :disabled="!isEditing"
              @input="checkStrength"
              :class="{ 'input-error': toast.error && !password }"
            />
            <i
              class="fa-regular fa-eye"
              :style="{ color: showPassword ? 'blue' : '#555' }"
              @click="showPassword = !showPassword"
            ></i>
          </div>
        </div>
        
        <div class="strength">
        <div class="bar" :style="{ width: strength * 25 + '%' }" :class="strengthColor"></div>
        <span class="strengthLabel">{{ strengthLabel }}</span>
      </div>
      </div>
      
    </div>

    <transition name="toast-fade">
      <div v-if="toast.show" class="toast" :class="{ error: toast.error }">
        {{ toast.message }}
        <span class="close" @click="toast.show = false">&times;</span>
      </div>
    </transition>
    
  </div>

</template>





<script>
import zxcvbn from 'zxcvbn'
import md5 from 'blueimp-md5' // Add this line

export default {
  name: 'AccountPanel',
  data() {
    return {
      firstName: 'Robert',
      lastName: 'Downey Jr.',
      email: 'mrvgatus@gmail.com',
      password: '@aa2dfsdf3##dsafds',
      showPassword: false,
      strength: 0,
      isEditing: false,
      toast: {
        show: false,
        message: '',
        error: false
      }
    }
  },
  computed: {
    gravatarUrl() {
      const hash = this.hashEmail(this.email.trim().toLowerCase())
      return `https://www.gravatar.com/avatar/${hash}?s=512?d=identicon`
    },
    strengthLabel() {
      return ['Very Weak', 'Weak', 'Moderate', 'Strong', 'Very Strong'][this.strength]
    },
    strengthColor() {
      return ['weak', 'weak', 'moderate', 'strong', 'strong'][this.strength]
    }
  },
  mounted() {
    this.checkStrength()
  },
  methods: {
    hashEmail(email) {
      return md5(email) // Use blueimp-md5
    },
    
    validateEmail(email) {
    var re = /\S+@\S+\.\S+/;
    return re.test(email);
    },

    checkStrength() {
      this.strength = zxcvbn(this.password).score
    },

    saveChanges() {
      if (!this.firstName || !this.lastName || !this.email) {
        this.toast.message = 'Please fill in all required fields.'
        this.toast.show = true
        this.toast.error = true
        return
      }

      if (!this.validateEmail(this.email)) {
        this.toast.message = 'Please enter a valid email address.'
        this.toast.show = true
        this.toast.error = true
        return
      }

      this.isEditing = false
      this.toast.message = 'Changes saved successfully!'
      this.toast.show = true
      this.toast.error = false
      setTimeout(() => (this.toast.show = false), 2000)
    }
  }
}
</script>

<style lang="scss">

.account-panel {
  background: #f2f2f2;
  width: 400px;
  max-width: 400px;
  border: 1px solid #ccc;
  border-radius: 50px;
  color: #333;
  padding-bottom: 2rem;

  .actions {
    position: relative;
    z-index: 9;
    button {
      position: absolute;
      top: 0.5rem;
      right: 1rem;
      background: transparent;
      border: none;
      cursor: pointer;
      font-size: 1.2rem;
      color: #fff;
      outline: none;
      &:hover {
        color: #007bff;
      }
    }
  }

  .avatar-container {
    position: relative;
    text-align: center;
    margin: 0.3rem;

    .avatar {
      width: 100%;
      height: 100%;
      border-radius: 50px;
    }
  }

  .field {
    margin-top: 1rem;
    padding: 0 2rem;
    text-align: left;

    label{
      display: block;
      font-weight: bold;
      margin-bottom: 0.1rem;
      color: #555;
      font-size: 0.8rem;
    }

    input {
      width: 100%;
      box-sizing: border-box;
      padding: 0.5rem;
      margin-top: 0.25rem;
      margin-bottom: 0.5rem;
      background: #fff;
      color: #333;
      border-top: 0;
      border-left: 0;
      border-right: 0;
      border-bottom: 1px solid #f2f2f2;
      outline: none;

      &.input-error {
        border-bottom: 2px solid #dc3545; // red border for error
      }
    }
  }

  .nameContainer {
    .fullName {
      font-size: 1.2rem;
      margin-top: 1rem;
      font-weight: 900;
      text-transform: uppercase;
      .verified {
        color: green;
        margin-left: 0.5rem;
      }
    }
  }
  
  .emailContainer {
    .email-icon {
      color: #ffc300;
      font-size: 0.8rem;
      margin-top: 0.5rem;
    }
    .emailAddress {
      font-size: 0.9rem;
      color: #555;
      margin-top: 0.5rem;
    }
  }
  
  .passwordContainer {
    .field-group {
      position: relative;
      display: flex;
      align-items: center;

      .input-icon-wrapper {
        position: relative;
        flex: 1;

        input {
          padding-right: 2rem; // Space for the icon
        }

        i {
          position: absolute;
          right: 0.5rem;
          top: 50%;
          transform: translateY(-50%);
          cursor: pointer;
          color: #555;
        }
      }
    }
  }

  .tooltip {
    position: absolute;
    top: -1.5rem;
    left: 50%;
    transform: translateX(-50%);
    background: #333;
    color: white;
    padding: 0.25rem 0.5rem;
    font-size: 0.75rem;
    border-radius: 3px;
    white-space: nowrap;
    opacity: 0.8;
  }

  .toggle {
    font-size: 0.8rem;
    color: blue;
    cursor: pointer;
  }

  .strength {
    display: flex;
    align-items: center;
    gap: 0.5rem;

    .bar {
      height: 5px;
      background: #ccc;
      flex: 1;
    }

    .weak {
      background: red;
      color: red;
    }

    .moderate {
      background: orange;
      color: orange;
    }

    .strong {
      background: green;
      color: green;
    }
    .strengthLabel {
      color: #333;
    }
  }

  .actions {
    text-align: right;

    button {
      padding: 0.5rem 1rem;
      cursor: pointer;
    }
  }

  .toast {
    position: fixed;
    top: 20px;
    right: 20px;
    background: #28a745;
    color: white;
    padding: 0.75rem 1.25rem;
    border-radius: 4px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);

      &.error {
        background: #dc3545; // red for error
      }

      .close {
        margin-left: 1rem;
        cursor: pointer;
      }
  }

  .toast-fade-enter-active,
  .toast-fade-leave-active {
    transition: opacity 0.5s;
  }
  .toast-fade-enter-from,
  .toast-fade-leave-to {
    opacity: 0;
  }
  .toast-fade-enter-to,
  .toast-fade-leave-from {
    opacity: 1;
  }
}
</style>
