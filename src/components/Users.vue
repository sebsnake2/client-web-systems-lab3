<script setup lang="ts">
import { computed, ref } from "vue";
import userData from "../data/user.json";

defineOptions({
  name: "UsersList",
});

interface User {
  id: number;
  gender: string;
  name: {
    title: string;
    first: string;
    last: string;
  };
  location: {
    street: {
      number: number;
      name: string;
    };
    city: string;
    state: string;
    country: string;
    postcode: number;
    timezone: {
      offset: string;
      description: string;
    };
  };
  email: string;
  dob: {
    date: string;
    age: number;
  };
  phone: string;
  cell: string;
  picture: string;
  hobbies: string[];
  details: string;
}

const user: User = userData;

const showDetails = ref(true);

const ageClass = computed(() => {
  const age = user.dob.age;

  if (age < 18) {
    return "minor";
  }

  if (age <= 30) {
    return "young";
  }

  if (age <= 50) {
    return "adult";
  }

  return "senior";
});

function toggleDetails(): void {
  showDetails.value = !showDetails.value;
}
</script>

<template>
  <article class="user-card" :class="ageClass">
    <section class="profile">
      <img
        class="profile__image"
        :src="user.picture"
        :alt="`${user.name.first} ${user.name.last}`"
      />

      <h1 class="profile__name">
        {{ user.name.title }}
        {{ user.name.first }}
        {{ user.name.last }}
      </h1>

      <div class="profile__summary">
        <span>{{ user.gender }}</span>

        <span v-if="user.dob.age > 18"> {{ user.dob.age }} years </span>
      </div>

      <p>
        {{ user.location.city }}, {{ user.location.state }},
        {{ user.location.country }}
      </p>

      <p>{{ user.email }}</p>
      <p>{{ user.phone }}</p>
      <p>{{ user.cell }}</p>
    </section>

    <section class="information">
      <button class="details-toggle" type="button" @click="toggleDetails">
        <span>About me</span>
        <span>{{ showDetails ? "▲" : "▼" }}</span>
      </button>

      <div v-show="showDetails" class="details">
        <p>{{ user.details }}</p>
      </div>

      <section class="info-block">
        <h2>Personal Information</h2>

        <div class="info-row">
          <span>Full name</span>
          <strong>
            {{ user.name.title }}
            {{ user.name.first }}
            {{ user.name.last }}
          </strong>
        </div>

        <div class="info-row">
          <span>Gender</span>
          <strong>{{ user.gender }}</strong>
        </div>

        <div class="info-row">
          <span>Date of birth</span>
          <strong>
            {{ new Date(user.dob.date).toLocaleDateString() }}
          </strong>
        </div>

        <div class="info-row">
          <span>Email</span>
          <strong>{{ user.email }}</strong>
        </div>

        <div class="info-row">
          <span>Phone</span>
          <strong>{{ user.phone }}</strong>
        </div>

        <div class="info-row">
          <span>Cell</span>
          <strong>{{ user.cell }}</strong>
        </div>
      </section>

      <section class="info-block">
        <h2>Location</h2>

        <div class="info-row">
          <span>Street</span>
          <strong>
            {{ user.location.street.number }}
            {{ user.location.street.name }}
          </strong>
        </div>

        <div class="info-row">
          <span>City</span>
          <strong>{{ user.location.city }}</strong>
        </div>

        <div class="info-row">
          <span>State</span>
          <strong>{{ user.location.state }}</strong>
        </div>

        <div class="info-row">
          <span>Country</span>
          <strong>{{ user.location.country }}</strong>
        </div>

        <div class="info-row">
          <span>Postcode</span>
          <strong>{{ user.location.postcode }}</strong>
        </div>

        <div class="info-row">
          <span>Timezone</span>
          <strong>
            {{ user.location.timezone.offset }}
            ({{ user.location.timezone.description }})
          </strong>
        </div>
      </section>

      <section class="info-block">
        <h2>Hobbies</h2>

        <div class="hobbies">
          <span v-for="hobby in user.hobbies" :key="hobby" class="hobby">
            {{ hobby }}
          </span>
        </div>
      </section>
    </section>
  </article>
</template>

<style scoped>
.user-card {
  display: grid;
  grid-template-columns: 340px 1fr;
  width: min(100%, 980px);
  margin: 0 auto;
  overflow: hidden;
  border: 2px solid transparent;
  border-radius: 18px;
  background: #ffffff;
  box-shadow: 0 12px 35px rgb(25 44 85 / 10%);
}

.user-card.minor {
  border-color: #f0b2b2;
}

.user-card.young {
  border-color: #b8cff7;
}

.user-card.adult {
  border-color: #a7d9bd;
}

.user-card.senior {
  border-color: #d7c2ef;
}

.profile {
  padding: 28px;
  border-right: 1px solid #e5eaf2;
}

.profile__image {
  width: 100%;
  aspect-ratio: 1 / 0.86;
  border-radius: 14px;
  object-fit: cover;
}

.profile__name {
  margin: 20px 0 12px;
  color: #172548;
  font-size: 30px;
}

.profile__summary {
  display: flex;
  gap: 24px;
  margin-bottom: 22px;
  color: #546585;
}

.profile p {
  margin: 13px 0;
  color: #52617d;
}

.information {
  padding: 28px;
}

.details-toggle {
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 15px 18px;
  border: 0;
  border-radius: 12px;
  background: #f2f6fc;
  color: #172548;
  font-weight: 700;
}

.details {
  margin-top: 12px;
  padding: 14px 18px;
  border-radius: 10px;
  background: #f8faff;
  color: #52617d;
}

.info-block {
  padding: 22px 4px;
  border-bottom: 1px solid #e5eaf2;
}

.info-block:last-child {
  border-bottom: 0;
}

.info-block h2 {
  margin: 0 0 18px;
  color: #172548;
  font-size: 18px;
}

.info-row {
  display: grid;
  grid-template-columns: 160px 1fr;
  gap: 16px;
  margin: 10px 0;
  color: #5a6988;
}

.info-row strong {
  color: #273756;
  font-weight: 500;
}

.hobbies {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.hobby {
  padding: 7px 13px;
  border-radius: 999px;
  background: #edf4ff;
  color: #3568b8;
  font-size: 14px;
}

@media (max-width: 760px) {
  .user-card {
    grid-template-columns: 1fr;
  }

  .profile {
    border-right: 0;
    border-bottom: 1px solid #e5eaf2;
  }

  .info-row {
    grid-template-columns: 1fr;
    gap: 4px;
  }
}
</style>
