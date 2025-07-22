<template>
  <div class="page">
    <div class="calculator">
      <h2>🎓 GPA Calculator (100 Marks per Subject)</h2>

      <div class="input-section">
        <div v-for="(subject, index) in subjects" :key="subject.name" class="input-group">
          <label>{{ subject.name }} ({{ subject.credits }} Cr):</label>
          <input
            type="number"
            v-model.number="subjects[index].marks"
            min="0"
            max="100"
            placeholder="Marks out of 100"
            @focus="clearZero($event)"
          />
        </div>

        <div class="input-group">
          <label>Previous GPA:</label>
          <input type="number" v-model.number="previousGPA" step="0.01" min="0" max="4" placeholder="e.g. 3.2" />
        </div>

        <div class="input-group">
          <label>Previous Credit Hours:</label>
          <input type="number" v-model.number="previousCredits" min="0" placeholder="e.g. 15" />
        </div>

        <button @click="calculateGPA">Calculate New GPA</button>
      </div>

      <div v-if="newGPA !== null" class="result">
        <p><strong>New GPA:</strong> {{ newGPA.toFixed(2) }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      subjects: [
        { name: "SE", credits: 3, marks: 0 },
        { name: "OOP", credits: 4, marks: 0 },
        { name: "Calculus", credits: 3, marks: 0 },
        { name: "DLD", credits: 3, marks: 0 },
        { name: "Database", credits: 4, marks: 0 }
      ],
      previousGPA: 0,
      previousCredits: 0,
      newGPA: null
    };
  },
  methods: {
    clearZero(event) {
      if (parseFloat(event.target.value) === 0) {
        event.target.value = "";
      }
    },
    getGradePoint(marks) {
      if (marks >= 85) return 4.0;
      if (marks >= 80) return 3.7;
      if (marks >= 75) return 3.3;
      if (marks >= 70) return 3.0;
      if (marks >= 65) return 2.7;
      if (marks >= 60) return 2.3;
      if (marks >= 50) return 2.0;
      return 0.0;
    },
    calculateGPA() {
      let currentQualityPoints = 0;
      let currentCredits = 0;

      this.subjects.forEach(subject => {
        const gradePoint = this.getGradePoint(subject.marks);
        currentQualityPoints += gradePoint * subject.credits;
        currentCredits += subject.credits;
      });

      const previousQualityPoints = this.previousGPA * this.previousCredits;
      const totalQualityPoints = currentQualityPoints + previousQualityPoints;
      const totalCredits = currentCredits + this.previousCredits;

      this.newGPA = totalCredits > 0 ? totalQualityPoints / totalCredits : 0;
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
  width: 180px;
  color: #444;
}

input {
  padding: 8px 12px;
  width: 120px;
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
