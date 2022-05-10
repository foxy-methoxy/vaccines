<template>
  <div class="container">
    <div class="navigation">
      <label for="pl" :class="{ checked: locale === 'pl' }">PL</label>
      <input type="radio" id="pl" value="pl" v-model="locale" />
      <label for="en" :class="{ checked: locale === 'en' }">EN</label>
      <input type="radio" id="en" value="en" v-model="locale" />
    </div>
    <div class="header">
      <h1 class="title">{{ t("title") }}</h1>
      <p class="description">{{ t("description") }}</p>
    </div>

    <div class="vaccines-list">
      <ul>
        <li
          v-for="(vaccine, index) in vaccines"
          :key="index"
          @click="scroll(vaccine.name)"
        >
          {{ vaccine.name }}
        </li>
      </ul>
    </div>

    <div
      class="vaccine-description"
      v-for="(vaccine, index) in vaccines"
      :key="index"
      :id="vaccine.name"
    >
      <div>
        <h2>{{ vaccine.name }}</h2>
        <p class="description">
          {{ t(`vaccines.${vaccine.name}.description`) }}
        </p>
      </div>
      <h3>{{ t(`vaccines.${vaccine.name}.ingredients.name`) }}</h3>
      <div>
        <h4>
          {{ t(`vaccines.${vaccine.name}.ingredients.active-substance.name`) }}
        </h4>
        <p class="active-substance">
          {{ t(`vaccines.${vaccine.name}.ingredients.active-substance.value`) }}
        </p>
      </div>
      <div>
        <h4>{{ t(`vaccines.${vaccine.name}.ingredients.excipients.name`) }}</h4>
        <ul>
          <li v-for="(i, index) in vaccine.additionalCount" :key="index">
            {{
              t(`vaccines.${vaccine.name}.ingredients.excipients.options.${i}`)
            }}
          </li>
        </ul>
      </div>

      <div class="vaccine-warnings" v-if="vaccine.warnings">
        <p>{{ t(`vaccines.${vaccine.name}.warnings.before`) }}</p>
        <ul>
          <li v-for="(i, index) in vaccine.warnings" :key="index">
            {{ t(`vaccines.${vaccine.name}.warnings.list.${i}`) }}
          </li>
        </ul>
        <p>{{ t(`vaccines.${vaccine.name}.warnings.after`) }}</p>
      </div>
    </div>

    <div class="summary">
      {{ t("summary") }}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, ref } from "vue";
import { useI18n } from "vue-i18n";

export default defineComponent({
  name: "APP",
  setup() {
    const { t, locale } = useI18n({
      locale: "pl",
      useScope: "global",
    });

    const vaccines = computed(() => [
      {
        name: "Pfizer / BioNtech",
        additionalCount: 10,
        warnings: 0,
      },
      {
        name: "AstraZeneca",
        additionalCount: 8,
        warnings: 0,
      },
      {
        name: "Johnson&Johnson",
        additionalCount: 9,
        warnings: 7,
      },
      {
        name: "Moderna",
        additionalCount: 9,
        warnings: 0,
      },
    ]);

    const element = ref("");

    const scroll = (vaccine: string) => {
      console.log("scroll");
      console.log(document.getElementById(vaccine));
      document.getElementById(vaccine)?.scrollIntoView();
    };

    return { t, locale, vaccines, scroll };
  },
});
</script>
<style lang="scss">
@font-face {
  font-family: "Quicksand";
  src: local("Quicksand"),
    url(./fonts/static/Quicksand-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "QuicksandBold";
  src: local("Quicksand"),
    url(./fonts/static/Quicksand-Bold.ttf) format("truetype");
}

@font-face {
  font-family: "QuicksandSemiBold";
  src: local("Quicksand"),
    url(./fonts/static/Quicksand-SemiBold.ttf) format("truetype");
}

body {
  font-family: Quicksand, sans-serif;
  background-image: linear-gradient(225deg, #020024, #686c77, #bcbdb6);
  color: white;
}

.container {
  padding: 0 4rem;
}

.navigation {
  display: flex;
  justify-content: flex-end;
  padding-top: 2rem;

  input {
    visibility: hidden;
  }

  label {
    border: 1px solid white;
    border-radius: 5px;
    display: block;
    cursor: pointer;
    padding: 5px 10px;
    opacity: 0.7;

    &.checked {
      opacity: 1;
    }
  }
}

.header {
  display: flex;
  justify-content: space-around;
  column-gap: 6rem;
  margin-top: 4rem;
  min-height: 250px;
  border-bottom: 1px solid white;
  padding-bottom: 4rem;

  h1.title {
    font-size: 38px;
  }

  .title,
  .description {
    display: flex;
    margin: 0;
  }

  .title {
    width: 100%;
    line-height: 60px;
    align-items: flex-start;
    padding: 2rem;
  }

  .description {
    width: 90%;
    align-items: flex-end;
  }
}

.vaccines-list {
  display: flex;
  justify-content: center;
  margin-top: 4rem;

  ul {
    list-style-type: none;
    display: flex;
    column-gap: 4rem;
    padding: 1rem 0 3rem 0;
    border-bottom: 1px solid white;

    li {
      font-size: 34px;
      text-transform: uppercase;
      transition: 2s ease-out 40ms;
      cursor: pointer;
      position: relative;
      padding: 1rem;

      &:hover {
        color: deeppink;
      }
    }
  }
}

.vaccine-description {
  margin-top: 4rem;
  display: flex;
  flex-direction: column;

  h3 {
    font-size: 30px;
  }

  div:last-of-type {
    display: flex;
    align-items: center;
    margin-top: 3rem;
    border-bottom: 1px solid white;
    padding-bottom: 3rem;

    li {
      padding: 0.25rem 0;
    }
  }

  div {
    display: flex;
    column-gap: 4rem;

    h2,
    h4 {
      width: 40%;
    }

    h2 {
      font-size: 48px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid white;
      border-radius: 10px;
      padding: 2rem 0;
    }
  }

  h2 {
    text-transform: uppercase;
  }

  h4 {
    font-size: 18px;
    align-self: flex-start;
  }

  .description {
    width: 60%;
    align-items: center;
    display: flex;
  }
}

.summary {
  padding: 3rem 0;
}

.vaccine-warnings {
  position: relative;
  padding: 3rem;

  &::before {
    position: absolute;
    left: 15%;
    top: 0;
    content: "";
    width: 70%;
    border-top: 1px solid white;
  }
}
</style>
