<template>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <h1 class="text-3xl font-bold mb-4">Club ABC</h1>
    <table class="table-fixed w-full">
      <thead>
        <tr>
          <th
            v-for="(header, index) in [
              'Event Name',
              'Date',
              'Time',
              'Location',
            ]"
            :key="index"
            class="px-4 py-2"
          >
            {{ header }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(event, rowIndex) in eventData"
          :key="rowIndex"
          class="hover:bg-gray-100"
        >
          <td class="px-4 py-2">{{ event.name }}</td>
          <td class="px-4 py-2">{{ event.date }}</td>
          <td class="px-4 py-2">{{ event.time }}</td>
          <td class="px-4 py-2">{{ event.location }}</td>
          <td class="px-4 py-2">
            <button
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-lg"
              @click="editEvent(rowIndex)"
            >
              Edit
            </button>
            <button
              class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-lg"
              @click="removeEvent(rowIndex)"
            >
              Remove
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="mt-6">
      <h2 class="text-2xl font-bold mb-2">Add Event</h2>
      <form @submit.prevent="addEvent">
        <div class="flex flex-col">
          <label for="eventName" class="mb-2">Event Name:</label>
          <input
            type="text"
            id="eventName"
            v-model="newEvent.name"
            class="px-4 py-2 rounded-lg"
            required
          />
        </div>
        <div class="flex flex-col">
          <label for="eventDate" class="mb-2">Date:</label>
          <input
            type="date"
            id="eventDate"
            v-model="newEvent.date"
            class="px-4 py-2 rounded-lg"
            required
          />
        </div>
        <div class="flex flex-col">
          <label for="eventTime" class="mb-2">Time:</label>
          <input
            type="time"
            id="eventTime"
            v-model="newEvent.time"
            class="px-4 py-2 rounded-lg"
            required
          />
        </div>
        <div class="flex flex-col">
          <label for="eventLocation" class="mb-2">Location:</label>
          <input
            type="text"
            id="eventLocation"
            v-model="newEvent.location"
            class="px-4 py-2 rounded-lg"
            required
          />
        </div>
        <button
          type="submit"
          class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded-lg"
        >
          Add
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useLoginInfo } from "../stores/loginInfo";
import { useRouter } from "vue-router";
import axios from "axios";

const loginInfo = useLoginInfo();
const router = useRouter();
const eventData = ref([
  //TEMPORARY PLACEHOLDER
  { name: "Meeting", date: "2023-10-01", time: "10:00", location: "Room 101" },
  { name: "Workshop", date: "2023-10-05", time: "14:00", location: "Lab 202" },
]);
const newEvent = ref({
  name: "",
  date: "",
  time: "",
  location: "",
});

const spreadsheetId = "1qG5AABVm3aLNkJjxizqjNjyE5jvyyvRZER8Icap4bLs";
const sheetName = "ClubData";

async function fetchSpreadsheetData() {
  const range = `${sheetName}!A1:Z100`;
  const apiKey = "";
  const url = `https://sheets.googleapis.com/v4/spreadsheets/${spreadsheetId}/values/${range}?key=${apiKey}`;

  try {
    const response = await axios.get(url);
    eventData.value = response.data.values;
  } catch (error) {
    console.error("Error fetching data from Google Sheets:", error);
  }
}

function editEvent(rowIndex) {
  router.push({
    name: "EditEvent",
    params: { rowIndex: rowIndex },
  });
}

function removeEvent(rowIndex) {
  const url = `https://sheets.googleapis.com/v4/spreadsheets/${spreadsheetId}/values/${sheetName}!A${
    rowIndex + 1
  }:Z${rowIndex + 1}?key=${apiKey}`;

  try {
    axios.delete(url);
    eventData.value.splice(rowIndex, 1);
  } catch (error) {
    console.error("Error removing data from Google Sheets:", error);
  }
}

function addEvent() {
  const url = `https://sheets.googleapis.com/v4/spreadsheets/${spreadsheetId}/values/${sheetName}!A1:Z1?valueInputOption=USER_ENTERED&key=${apiKey}`;

  try {
    axios.post(url, [Object.values(newEvent.value)]);
    eventData.value.push({ ...newEvent.value });
    newEvent.value = {
      name: "",
      date: "",
      time: "",
      location: "",
    };
  } catch (error) {
    console.error("Error adding data to Google Sheets:", error);
  }
}
onMounted(fetchSpreadsheetData);
</script>
