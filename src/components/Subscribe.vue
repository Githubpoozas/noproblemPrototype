<template>
  <section class="subscribe__container">
    <h1 class="subscribe__header">Newsletter</h1>
    <p lass="subscribe__para">
      Subscribe to our Newsletter and we will send you news and information
      about new trends every week.
    </p>
    <form class="subscribe__form">
      <fieldset>
        <EmailInput
          idName="subEmail"
          :submitNull="email.submitNull"
          autocomplete="on"
          @emitEmail="email.input = $event"
        ></EmailInput>

        <div class="subscribe__form__interest">
          <legend class="subscribe__form__interest__legend">
            Sections of your interest
          </legend>
          <input
            class="subscribe__form__interest__input"
            type="checkbox"
            name="woman"
            value="woman"
            id="woman"
            v-model="woman"
          />
          <label class="subscribe__form__interest__checkbox__label" for="woman">
            <span class="subscribe__form__interest__checkbox"
              ><i
                class="subscribe__form__interest__checkbox-icon fas fa-check"
              ></i
            ></span>
            Woman
          </label>
          <input
            class="subscribe__form__interest__input"
            type="checkbox"
            name="man"
            value="man"
            id="man"
            v-model="man"
          />
          <label class="subscribe__form__interest__checkbox__label" for="man">
            <span class="subscribe__form__interest__checkbox"
              ><i
                class="subscribe__form__interest__checkbox-icon fas fa-check"
              ></i
            ></span>
            Man
          </label>
          <input
            class="subscribe__form__interest__input"
            type="checkbox"
            name="kids"
            value="kids"
            id="kids"
            v-model="kids"
          />
          <label class="subscribe__form__interest__checkbox__label" for="kids">
            <span class="subscribe__form__interest__checkbox"
              ><i
                class="subscribe__form__interest__checkbox-icon fas fa-check"
              ></i
            ></span>
            Kids
          </label>
        </div>
        <div class="subscribe__form__privacy">
          <input
            class="subscribe__form__privacy__input"
            type="checkbox"
            name="privacy"
            value="privacy"
            id="privacy"
            v-model="privacy"
          />
          <label
            class="subscribe__form__privacy__checkbox__label"
            for="privacy"
          >
            <span class="subscribe__form__privacy__checkbox"
              ><i
                class="subscribe__form__privacy__checkbox-icon fas fa-check"
              ></i
            ></span>
            I accept the
            <a class="subscribe__form__privacy__link" href="#"
              >privacy statement</a
            >
          </label>
        </div>
        <div class="subscribe__form__submit">
          <button
            class="subscribe__form__submit-btn btn-main"
            @click.stop.prevent="subscribe"
          >
            SUBSCRIBE
          </button>
        </div>
      </fieldset>
    </form>

    <div class="modal__container" v-show="modalShow" @click="modalShow = false">
      <div class="modal__box">
        <i class="fas fa-exclamation modal__icon"></i>
        <h3 class="modal__warning">WARNING</h3>
        <p class="modal__prompt">
          {{ prompt }}
        </p>
        <button
          type="button"
          class="modal__btn btn-main"
          @click="modalShow = false"
        >
          ACCEPT
        </button>
      </div>
    </div>
    
  </section>
</template>

<script>
import EmailInput from "@/components/EmailInput.vue";

export default {
  data() {
    return {
      email: {
        input: "",
        submitNull: false,
      },
      woman: false,
      man: false,
      kids: false,
      privacy: false,
      modalShow: false,
      prompt: "",
    };
  },
  components: {
    EmailInput,
  },
  methods: {
    subscribe() {
      if (!this.email.input) {
        document.getElementById("subEmail").scrollIntoView({
          behavior: "smooth",
          block: "center",
          inline: "nearest",
        });
        this.email.submitNull = true;
      } else if (!this.woman && !this.man && !this.kids) {
        this.prompt = "You must choose at least one of the options.";
        this.modalShow = true;
      } else if (!this.privacy) {
        this.prompt = "You must accept the privacy policy.";
        this.modalShow = true;
      } else {
        console.log(`${this.email.input} subscribe`);
        this.$router.push("/confirm/subscribe");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.subscribe {
  &__container {
    padding-top: 3px;
  }

  &__header {
    padding: 18px 0 8px;
  }

  &__form {
    width: 47.5%;
    &__email {
      &__typing {
        &__container {
          padding-bottom: 5px;
          padding-top: 10px;
          width: 100%;
          max-width: 350px;
        }
      }
    }

    &__interest {
      padding-bottom: 5px;
      padding-top: 10px;
      position: relative;
      &__legend {
        font-size: 1.5rem;
        font-weight: 700;
        margin-bottom: 20px;
      }
    }
    &__privacy {
      margin-top: 10px;
      &__link {
        text-decoration: underline;
      }
    }
    &__submit {
      margin-top: 50px;
    }
  }
}
</style>

<style lang="scss">
h1[class$="__header"] {
  font-size: 2.2rem;
  font-weight: 700;
}
p[class$="__para"] {
  letter-spacing: 0.2px;
  line-height: 1.5;
}
form[class$="__form"] {
  margin-top: 40px;
  font-size: 1.4rem;
}
div[class$="__email"] {
  padding-top: 10px;
  padding-bottom: 5px;
  width: 100%;
  max-width: 350px;
}

label[class*="__typing__label"] {
  position: absolute;
  opacity: 0.6;
  font-size: 1.6rem;
  pointer-events: none;
  transition: all 0.2s;
}
label[class*="__label"] {
  &.moveTop {
    transform: translateY(-25px) scale(0.8);
    margin-top: 0;
    overflow: visible;
    margin-left: -10px;
    opacity: 0.4;
  }
}

input[class*="__typing__input"],
select[class*="__select__input"],
textarea[class*="__textarea__input"] {
  background: none;
  border: none;
  border-radius: 0;
  border-bottom: 2px solid #cccccc;
  font-size: 1.6rem;
  letter-spacing: 0.2px;
  line-height: 15px;
  padding: 0 0 5px;
  width: 100%;

  &:focus {
    outline: none;
  }
  &.inputFocus {
    border-color: black;
  }
  &.inputError {
    border-color: red;
  }
}

textarea[class*="__textarea__input"] {
  resize: none;
  overflow-y: hidden;
  line-height: 20px;

}

p[class*="__input-warning"] {
  color: red;
  margin-top: 10px;
  font-size: 1.2rem;
  visibility: hidden;
  &.warningVisible {
    visibility: visible;
  }
}

label[class*="__checkbox__label"] {
  margin-bottom: 20px;
}

button[class*="__submit-btn"] {
  height: 40px;
  min-width: 201px;
  font-size: 14px;
  letter-spacing: 0.2px;
  font-weight: 400;
}
</style>
