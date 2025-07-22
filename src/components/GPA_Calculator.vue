<template>
  <div class="gpa-calculator">
    <h2>🎓 GPA Calculator</h2>

    <div class="input-group" v-for="subject in allSubjects" :key="subject.name">
      <label>
        <input
          type="checkbox"
          v-model="subject.selected"
        />
        {{ subject.name }} ({{ subject.credits }} Cr)
      </label>

      <input
        v-if="subject.selected"
        type="number"
        v-model.number="subject.marks"
        placeholder="Marks out of 100"
        min="0"
        max="100"
      />
    </div>

    <div class="input-group">
      <label>Previous GPA:</label>
      <input type="number" v-model.number="previousGPA" step="0.01" min="0" max="4" />
    </div>

    <button @click="calculateGPA">Calculate GPA</button>

    <div v-if="newGPA !== null" class="result">
      <p><strong>📊 New GPA:</strong> {{ newGPA.toFixed(2) }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GpaCalculator',
  data() {
    return {
      allSubjects: [
        { name: 'SE', credits: 3, selected: false, marks: 0 },
        { name: 'OOP', credits: 4, selected: false, marks: 0 },
        { name: 'Calculus', credits: 3, selected: false, marks: 0 },
        { name: 'DLD', credits: 3, selected: false, marks: 0 },
        { name: 'Database', credits: 4, selected: false, marks: 0 }
      ],
      previousGPA: 0,
      previousCredits: 31,
      newGPA: null
    };
  },
  methods: {
    getGradePoint(marks) {
      if (marks >= 85) return 4.0;
      if (marks >= 75) return 3.7;
      if (marks >= 65) return 3.3;
      if (marks >= 55) return 3.0;
      if (marks >= 45) return 2.7;
      if (marks >= 40) return 2.0;
      return 0.0;
    },
    calculateGPA() {
      let currentQP = 0;
      let currentCredits = 0;

      this.allSubjects.forEach(sub => {
        if (sub.selected) {
          const gp = this.getGradePoint(sub.marks);
          currentQP += gp * sub.credits;
          currentCredits += sub.credits;
        }
      });

      const totalQP = (this.previousGPA * this.previousCredits) + currentQP;
      const totalCredits = this.previousCredits + currentCredits;

      this.newGPA = totalCredits > 0 ? totalQP / totalCredits : 0;
    }
  }
};
</script>

<style scoped>
.gpa-calculator {
  max-width: 500px;
  margin: 20px auto;
  padding: 25px;
  background-color: #f4f6f9;
  border-radius: 12px;
  box-shadow: 0 0 8px rgba(0,0,0,0.1);
  font-family: Arial, sans-serif;
}
h2 {
  text-align: center;
  margin-bottom: 20px;
}
.input-group {
  margin-bottom: 15px;
}
input[type="number"] {
  width: 100%;
  padding: 6px;
  margin-top: 5px;
}
button {
  width: 100%;
  background-color: #007bff;
  color: white;
  font-weight: bold;
  padding: 10px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
.result {
  margin-top: 20px;
  text-align: center;
  font-size: 1.2rem;
}
</style>
