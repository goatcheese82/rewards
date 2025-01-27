# WeeklyCalendar.vue
<template>
  <div class="calendar-container">
    <h2>Odin's Daily Rewards</h2>

    <button @click="print" class="print-btn">Print Calendar</button>

    <div class="calendar">
      <!-- Headers -->
      <div class="day-header" v-for="day in days" :key="day">
        {{ day }}
      </div>

      <!-- Calendar cells -->
      <div v-for="(day, index) in weekData" :key="index" class="day-cell">
        <div class="date">{{ day.date }}</div>
        <div class="options">
          <!-- Clothing Option -->
          <div class="option-group">
            <svg class="icon" viewBox="0 0 20 22">
              <path
                fill="currentColor"
                d="M16 2l4 4v1l-4 4v11H4V11L0 7V6l4-4h4l2 3h0l2-3z"
              />
            </svg>
            <div class="checkbox-pair">
              <label>
                <input
                  type="checkbox"
                  v-model="day.clothing.yes"
                  @change="uncheckPair('clothing', index, 'yes')"
                />
                Yes
              </label>
              <label>
                <input
                  type="checkbox"
                  v-model="day.clothing.no"
                  @change="uncheckPair('clothing', index, 'no')"
                />
                No
              </label>
            </div>
          </div>

          <!-- Follows Directions Option -->
          <div class="option-group">
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"
              />
            </svg>
            <div class="checkbox-pair">
              <label>
                <input
                  type="checkbox"
                  v-model="day.directions.yes"
                  @change="uncheckPair('directions', index, 'yes')"
                />
                Yes
              </label>
              <label>
                <input
                  type="checkbox"
                  v-model="day.directions.no"
                  @change="uncheckPair('directions', index, 'no')"
                />
                No
              </label>
            </div>
          </div>

          <!-- Kind Words Option -->
          <div class="option-group">
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M20 2H4c-1.1 0-2 .9-2 2v18l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 14H6l-2 2V4h16v12z"
              />
            </svg>
            <div class="checkbox-pair">
              <label>
                <input
                  type="checkbox"
                  v-model="day.kindWords.yes"
                  @change="uncheckPair('kindWords', index, 'yes')"
                />
                Yes
              </label>
              <label>
                <input
                  type="checkbox"
                  v-model="day.kindWords.no"
                  @change="uncheckPair('kindWords', index, 'no')"
                />
                No
              </label>
            </div>
          </div>

          <!-- Cleanup Option -->
          <div class="option-group">
            <svg class="icon" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M15 16h4v2h-4zm0-8h7v2h-7zm0 4h6v2h-6zM3 18c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V8H3v10zM14 5h-3l-1-1H6L5 5H2v2h12z"
              />
            </svg>
            <div class="checkbox-pair">
              <label>
                <input
                  type="checkbox"
                  v-model="day.cleanup.yes"
                  @change="uncheckPair('cleanup', index, 'yes')"
                />
                Yes
              </label>
              <label>
                <input
                  type="checkbox"
                  v-model="day.cleanup.no"
                  @change="uncheckPair('cleanup', index, 'no')"
                />
                No
              </label>
            </div>
          </div>
        </div>

        <!-- Notes Section -->
        <div class="notes-section">
          <p>Notes</p>
          <div class="ruled-line"></div>
          <div></div>
          <div class="ruled-line"></div>
          <div></div>
          <div class="ruled-line"></div>
          <div></div>
          <div class="ruled-line"></div>
        </div>
      </div>
    </div>
  </div>

  <div class="symbol-key">
    <h3>Behavior Key</h3>
    <div class="key-items">
      <div class="key-item">
        <svg class="icon" viewBox="0 0 20 22">
          <path
            fill="currentColor"
            d="M16 2l4 4v1l-4 4v11H4V11L0 7V6l4-4h4l2 3h0l2-3z"
          />
        </svg>
        <span>Keeps all clothing on</span>
      </div>

      <div class="key-item">
        <svg class="icon" viewBox="0 0 24 24">
          <path
            fill="currentColor"
            d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z"
          />
        </svg>
        <span>Follows Directions</span>
      </div>

      <div class="key-item">
        <svg class="icon" viewBox="0 0 24 24">
          <path
            fill="currentColor"
            d="M20 2H4c-1.1 0-2 .9-2 2v18l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 14H6l-2 2V4h16v12z"
          />
        </svg>
        <span>Uses Kind and Respectful words to teacher</span>
      </div>

      <div class="key-item">
        <svg class="icon" viewBox="0 0 24 24">
          <path
            fill="currentColor"
            d="M15 16h4v2h-4zm0-8h7v2h-7zm0 4h6v2h-6zM3 18c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V8H3v10zM14 5h-3l-1-1H6L5 5H2v2h12z"
          />
        </svg>
        <span>Cleans up after being asked</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeeklyCalendar",
  data() {
    return {
      days: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
      weekData: [],
    };
  },
  created() {
    this.initializeWeek();
  },
  methods: {
    initializeWeek() {
      const today = new Date();
      const currentDay = today.getDay();
      const mondayOffset = currentDay === 0 ? -6 : 1 - currentDay;
      const startOfWeek = new Date(today);
      startOfWeek.setDate(today.getDate() + mondayOffset);

      this.weekData = Array.from({ length: 5 }, (_, i) => {
        const date = new Date(startOfWeek);
        date.setDate(startOfWeek.getDate() + i);
        return {
          date: date.getDate(),
          clothing: { yes: false, no: false },
          directions: { yes: false, no: false },
          kindWords: { yes: false, no: false },
          cleanup: { yes: false, no: false },
        };
      });
    },
    uncheckPair(type, dayIndex, selected) {
      if (selected === "yes" && this.weekData[dayIndex][type].yes) {
        this.weekData[dayIndex][type].no = false;
      } else if (selected === "no" && this.weekData[dayIndex][type].no) {
        this.weekData[dayIndex][type].yes = false;
      }
    },
    print() {
      window.print();
    },
  },
};
</script>

<style scoped>
.calendar-container {
  margin: 20px auto;
  padding: 20px;
}

.print-btn {
  margin-bottom: 20px;
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.calendar {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
  border: 1px solid #ddd;
  padding: 10px;
}

.day-header {
  font-weight: bold;
  text-align: center;
  padding: 10px;
  background-color: #f5f5f5;
}

.day-cell {
  border: 1px solid #ddd;
  padding: 10px;
  min-height: 100px;
  display: flex;
  flex-direction: column;
}

.date {
  font-weight: bold;
  font-size: 2em;
  margin-bottom: 15px;
  text-align: right;
  border-bottom: 1px solid black;
}

.options {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.option-group {
  display: flex;
  align-items: center;
  gap: 15px;
}

.checkbox-pair label {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 0.9rem;
}

.icon {
  width: 20px;
  height: 20px;
  color: #666;
}

.notes-section {
  margin-top: auto;
  padding-top: 15px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.ruled-line {
  height: 1px;
  background: linear-gradient(to right, #ccc 50%, transparent 50%);
  background-size: 4px 1px;
  opacity: 0.5;
}

.symbol-key {
  margin: auto;
  margin-top: 30px;
  max-width: 800px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.symbol-key h3 {
  margin-bottom: 15px;
  color: #333;
}

.key-items {
  display: flex;
  gap: 30px;
  flex-wrap: wrap;
}

.key-item {
  display: flex;
  align-items: center;
  gap: 10px;
}

.key-item .icon {
  width: 24px;
  height: 24px;
}

.key-item span {
  font-size: 0.9rem;
  color: #444;
}

@media print {
  .print-btn {
    display: none;
  }

  .calendar-container {
    margin: 5%;
    padding: 0;
  }

  .calendar {
    border: none;
  }

  .option-group {
    break-inside: avoid;
  }

  .ruled-line {
    background: #ccc;
    opacity: 0.3;
  }

  .symbol-key {
    border: none;
    padding: 10px 0;
  }
}
</style>