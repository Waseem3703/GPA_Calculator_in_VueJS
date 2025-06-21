<template>
  <!-- Full‑page flex wrapper -->
  <div class="page">
    <div class="calculator">
      <h2>🎓 Weighted Grade Calculator</h2>

      <div class="input-section">
        <div v-for="(item, key) in scores" :key="key" class="section">
          <h3>{{ keyLabels[key] }}</h3>

          <!-- Repeating inputs (quizzes & assignments) -->
          <template v-if="Array.isArray(item)">
            <div
              v-for="(entry, index) in item"
              :key="index"
              class="input-group"
            >
              <label>Total {{ keyLabels[key] }} {{ index + 1 }}:</label>
              <input
                type="number"
                v-model.number="scores[key][index].total"
                min="0"
                placeholder="Total"
              />

              <label>Obtained:</label>
              <input
                type="number"
                v-model.number="scores[key][index].obtained"
                min="0"
                placeholder="Obtained"
              />
            </div>
          </template>

          <!-- Single inputs (midterm & final) -->
          <template v-else>
            <div class="input-group">
              <label>Total Marks:</label>
              <input
                type="number"
                v-model.number="scores[key].total"
                min="0"
                placeholder="Total"
              />
              <label>Obtained Marks:</label>
              <input
                type="number"
                v-model.number="scores[key].obtained"
                min="0"
                placeholder="Obtained"
              />
            </div>
          </template>
        </div>

        <button @click="calculate">Calculate Grade</button>
      </div>

      <div v-if="total !== null" class="result">
        <p><strong>Total Weighted Marks:</strong> {{ total.toFixed(2) }} / 100</p>
        <p><strong>Grade:</strong> {{ grade }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      scores: {
        quizzes: Array.from({ length: 4 }, () => ({ total: 0, obtained: 0 })),
        assignments: Array.from({ length: 4 }, () => ({ total: 0, obtained: 0 })),
        midterm: { total: 0, obtained: 0 },
        final: { total: 0, obtained: 0 }
      },
      weights: { quizzes: 12.5, assignments: 12.5, midterm: 25, final: 50 },
      keyLabels: { quizzes: "Quiz Marks", assignments: "Assignment Marks", midterm: "Midterm Marks", final: "Final Marks" },
      total: null,
      grade: ""
    };
  },
  methods: {
    calculate() {
      const avg = list =>
        list.reduce((sum, { total, obtained }) => sum + (total ? (obtained / total) * 100 : 0), 0) /
        list.length;

      const quizAvg = avg(this.scores.quizzes);
      const assignmentAvg = avg(this.scores.assignments);
      const midPct = this.scores.midterm.total
        ? (this.scores.midterm.obtained / this.scores.midterm.total) * 100
        : 0;
      const finalPct = this.scores.final.total
        ? (this.scores.final.obtained / this.scores.final.total) * 100
        : 0;

      this.total =
        (quizAvg * this.weights.quizzes +
          assignmentAvg * this.weights.assignments +
          midPct * this.weights.midterm +
          finalPct * this.weights.final) /
        100;

      this.grade =
        this.total >= 90
          ? "A"
          : this.total >= 80
          ? "B"
          : this.total >= 70
          ? "C"
          : this.total >= 60
          ? "D"
          : "F";
    }
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap");

.page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #f5f9ff;
}

.calculator {
  font-family: "Poppins", sans-serif;
  max-width: 700px;
  width: 100%;
  background: #ffffff;
  padding: 30px;
  border-radius: 18px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
  color: #333;
}

h2 {
  text-align: center;
  color: #007bff;
  margin-bottom: 25px;
}

h3 {
  text-align: center;
  margin-top: 20px;
  font-size: 18px;
  color: #333;
}

.input-section {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.input-group {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin: 10px 0;
  align-items: center;
}

label {
  font-size: 14px;
  width: 130px;
  color: #444;
}

input {
  padding: 8px 12px;
  width: 110px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 8px;
  transition: border-color 0.2s, box-shadow 0.2s;
}

input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 4px rgba(0, 123, 255, 0.5);
}

button {
  align-self: center;
  margin-top: 20px;
  padding: 12px 28px;
  font-size: 16px;
  background: #007bff;
  color: #fff;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background 0.3s;
}

button:hover {
  background: #0056b3;
}

.result {
  text-align: center;
  margin-top: 25px;
  padding: 15px;
  font-size: 18px;
  background: #eaf4ff;
  border-radius: 10px;
  color: #333;
}
</style>
