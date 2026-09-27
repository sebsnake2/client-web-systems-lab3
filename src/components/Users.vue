<script setup lang="ts">
import { computed, ref } from "vue";
import usersData from "../data/user.json";

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

type GenderFilter = "all" | "male" | "female";
type AgeFilter = "all" | "adult";
type SortType = "default" | "name-asc" | "name-desc" | "age-asc" | "age-desc";

const users: User[] = usersData;

const genderFilter = ref<GenderFilter>("all");
const ageFilter = ref<AgeFilter>("all");
const sortType = ref<SortType>("default");

const visibleDetails = ref<number[]>([]);

const filteredUsers = computed(() => {
  let result = [...users];

  if (genderFilter.value !== "all") {
    result = result.filter((user) => user.gender === genderFilter.value);
  }

  if (ageFilter.value === "adult") {
    result = result.filter((user) => user.dob.age >= 18);
  }

  switch (sortType.value) {
    case "name-asc":
      result.sort((a, b) => a.name.first.localeCompare(b.name.first));
      break;

    case "name-desc":
      result.sort((a, b) => b.name.first.localeCompare(a.name.first));
      break;

    case "age-asc":
      result.sort((a, b) => a.dob.age - b.dob.age);
      break;

    case "age-desc":
      result.sort((a, b) => b.dob.age - a.dob.age);
      break;

    default:
      break;
  }

  return result;
});

function getAgeClass(age: number): string {
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
}

function toggleDetails(userId: number): void {
  if (visibleDetails.value.includes(userId)) {
    visibleDetails.value = visibleDetails.value.filter((id) => id !== userId);
    return;
  }

  visibleDetails.value = [...visibleDetails.value, userId];
}

function isDetailsVisible(userId: number): boolean {
  return visibleDetails.value.includes(userId);
}

function clearFilters(): void {
  genderFilter.value = "all";
  ageFilter.value = "all";
  sortType.value = "default";
}
</script>

<template>
  <section class="users-page">
    <header class="page-header">
      <h1>Users</h1>
      <p>{{ filteredUsers.length }} users shown</p>
    </header>

    <div class="toolbar">
      <div class="control-group">
        <span class="control-label">Gender</span>

        <div class="buttons">
          <button
            type="button"
            :class="{ active: genderFilter === 'all' }"
            @click="genderFilter = 'all'"
          >
            Всі
          </button>

          <button
            type="button"
            :class="{ active: genderFilter === 'male' }"
            @click="genderFilter = 'male'"
          >
            Чоловіки
          </button>

          <button
            type="button"
            :class="{ active: genderFilter === 'female' }"
            @click="genderFilter = 'female'"
          >
            Жінки
          </button>
        </div>
      </div>

      <div class="control-group">
        <span class="control-label">Age</span>

        <div class="buttons">
          <button type="button" :class="{ active: ageFilter === 'all' }" @click="ageFilter = 'all'">
            Всі
          </button>

          <button
            type="button"
            :class="{ active: ageFilter === 'adult' }"
            @click="ageFilter = 'adult'"
          >
            18+
          </button>
        </div>
      </div>

      <div class="control-group">
        <span class="control-label">Sort</span>

        <div class="buttons">
          <button
            type="button"
            :class="{ active: sortType === 'name-asc' }"
            @click="sortType = 'name-asc'"
          >
            Ім'я ↑
          </button>

          <button
            type="button"
            :class="{ active: sortType === 'name-desc' }"
            @click="sortType = 'name-desc'"
          >
            Ім'я ↓
          </button>

          <button
            type="button"
            :class="{ active: sortType === 'age-asc' }"
            @click="sortType = 'age-asc'"
          >
            Вік ↑
          </button>

          <button
            type="button"
            :class="{ active: sortType === 'age-desc' }"
            @click="sortType = 'age-desc'"
          >
            Вік ↓
          </button>
        </div>
      </div>

      <button type="button" class="clear-button" @click="clearFilters">Очистити все</button>
    </div>

    <p v-if="filteredUsers.length === 0" class="empty-message">Список юзерів пустий.</p>

    <div v-else class="users-grid">
      <article
        v-for="user in filteredUsers"
        :key="user.id"
        class="user-card"
        :class="getAgeClass(user.dob.age)"
      >
        <div class="profile">
          <img
            class="profile__image"
            :src="user.picture"
            :alt="`${user.name.first} ${user.name.last}`"
          />

          <h2>
            {{ user.name.title }}
            {{ user.name.first }}
            {{ user.name.last }}
          </h2>

          <div class="profile__meta">
            <span>{{ user.gender }}</span>

            <span v-if="user.dob.age > 18"> {{ user.dob.age }} years </span>
          </div>

          <p>
            {{ user.location.city }},
            {{ user.location.country }}
          </p>

          <p>{{ user.email }}</p>
          <p>{{ user.phone }}</p>
        </div>

        <div class="information">
          <button type="button" class="details-toggle" @click="toggleDetails(user.id)">
            <span>About me</span>

            <span>
              {{ isDetailsVisible(user.id) ? "▲" : "▼" }}
            </span>
          </button>

          <div v-show="isDetailsVisible(user.id)" class="details">
            {{ user.details }}
          </div>

          <section class="info-block">
            <h3>Personal Information</h3>

            <div class="info-row">
              <span>Gender</span>
              <strong>{{ user.gender }}</strong>
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
            <h3>Location</h3>

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
          </section>

          <section class="info-block">
            <h3>Hobbies</h3>

            <div class="hobbies">
              <span v-for="hobby in user.hobbies" :key="hobby" class="hobby">
                {{ hobby }}
              </span>
            </div>
          </section>
        </div>
      </article>
    </div>
  </section>
</template>

<style scoped>
.users-page {
  width: min(100%, 1200px);
  margin: 0 auto;
}

.page-header {
  margin-bottom: 24px;
}

.page-header h1 {
  margin: 0;
  color: #172548;
  font-size: 36px;
}

.page-header p {
  margin: 6px 0 0;
  color: #697795;
}

.toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-end;
  gap: 18px;
  margin-bottom: 28px;
  padding: 20px;
  border-radius: 16px;
  background: #ffffff;
  box-shadow: 0 8px 24px rgb(25 44 85 / 8%);
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.control-label {
  color: #697795;
  font-size: 13px;
  font-weight: 600;
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}

.buttons button,
.clear-button {
  padding: 8px 13px;
  border: 1px solid #d8e1ef;
  border-radius: 9px;
  background: #ffffff;
  color: #344564;
  transition: 0.15s ease;
}

.buttons button:hover,
.buttons button.active {
  border-color: #4e7fd3;
  background: #edf4ff;
  color: #2865bd;
}

.clear-button {
  margin-left: auto;
}

.clear-button:hover {
  border-color: #cf6262;
  background: #fff0f0;
  color: #b73c3c;
}

.users-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 22px;
}

.user-card {
  overflow: hidden;
  border: 2px solid transparent;
  border-radius: 17px;
  background: #ffffff;
  box-shadow: 0 10px 28px rgb(25 44 85 / 9%);
}

.user-card.minor {
  border-color: #efb1b1;
}

.user-card.young {
  border-color: #b3cef7;
}

.user-card.adult {
  border-color: #a9d9bc;
}

.user-card.senior {
  border-color: #d2bdea;
}

.profile {
  padding: 22px;
}

.profile__image {
  width: 100%;
  aspect-ratio: 4 / 3;
  border-radius: 13px;
  object-fit: cover;
  object-position: center;
}

.profile h2 {
  margin: 16px 0 8px;
  color: #172548;
}

.profile__meta {
  display: flex;
  gap: 18px;
  margin-bottom: 16px;
  color: #63718d;
}

.profile p {
  margin: 7px 0;
  overflow-wrap: anywhere;
  color: #586783;
}

.information {
  padding: 0 22px 22px;
}

.details-toggle {
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 13px 15px;
  border: 0;
  border-radius: 10px;
  background: #f1f5fb;
  color: #172548;
  font-weight: 700;
}

.details {
  margin-top: 10px;
  padding: 13px 15px;
  border-radius: 10px;
  background: #f8faff;
  color: #596985;
}

.info-block {
  padding-top: 18px;
}

.info-block h3 {
  margin: 0 0 12px;
  color: #172548;
}

.info-row {
  display: grid;
  grid-template-columns: 100px 1fr;
  gap: 12px;
  margin: 7px 0;
  color: #65738f;
}

.info-row strong {
  overflow-wrap: anywhere;
  color: #34435f;
  font-weight: 500;
}

.hobbies {
  display: flex;
  flex-wrap: wrap;
  gap: 7px;
}

.hobby {
  padding: 6px 11px;
  border-radius: 999px;
  background: #edf4ff;
  color: #3268ba;
  font-size: 13px;
}

.empty-message {
  padding: 40px;
  border-radius: 16px;
  background: #ffffff;
  color: #697795;
  text-align: center;
}

@media (max-width: 850px) {
  .users-grid {
    grid-template-columns: 1fr;
  }

  .clear-button {
    margin-left: 0;
  }
}
</style>
