# Diag_App
import React, { useState } from "react";
import { View, Text, ScrollView, Button, TouchableOpacity } from "react-native";

const symptomsList = [
  "Sour Throat",
  "Cough",
  "Thorax Pain",
  "Fever Isolated",
  "Tropical Fever after stay",
  "Stomach Ache",
  "Nausea",
  "Diarrhea",
  "Pain urinating",
  "Toxics Consumption",
  "Yellowish",
  "Injury",
  "Skin injury (in Africa)"
];

const testSuggestions = {
  "Cough": "Pulmonary Kit (Influenza, Pneumococcus, Legionella)",
  "Thorax Pain": "Pulmonary Kit & Cardiac Pain Kit (Troponin, Ddimer)",
  "Fever Isolated": "Pulmonary Kit & Throat Kit (Strepto A)",
  "Tropical Fever after stay": "Tropical Kit (Dengue, Malaria)",
  "Stomach Ache": "Digestive Kit (Adeno, Rota virus, NoroVirus)",
  "Nausea": "Digestive Kit (Adeno, Rota virus, NoroVirus)",
  "Diarrhea": "Digestive Kit (Adeno, Rota virus, NoroVirus)",
  "Pain urinating": "Urinary Kit (White cells, Nitrite)",
  "Toxics Consumption": "Drug Kit (11 drugs)",
  "Yellowish": "Liver Function Test (Bilirubin, Hepatitis Panel)",
  "Injury": "Blood Clotting Test (PT/INR, D-Dimer)",
  "Skin injury (in Africa)": "Bacterial & Parasitic Infection Test (Leishmaniose, Mycobacterium)"
};

const kitColors = {
  "Pulmonary Kit": "#3498db",
  "Cardiac Pain Kit": "#e74c3c",
  "Throat Kit": "#9b59b6",
  "Tropical Kit": "#2ecc71",
  "Urinary Kit": "#f1c40f",
  "Digestive Kit": "#e67e22",
  "Drug Kit": "#c0392b",
  "Liver Function Test": "#16a085",
  "Blood Clotting Test": "#8e44ad",
  "Bacterial & Parasitic Infection Test": "#d35400"
};

export default function App() {
  const [selectedSymptoms, setSelectedSymptoms] = useState([]);
  const [recommendedTest, setRecommendedTest] = useState([]);

  const toggleSymptom = (symptom) => {
    setSelectedSymptoms((prev) =>
      prev.includes(symptom)
        ? prev.filter((s) => s !== symptom)
        : [...prev, symptom]
    );
  };

  const findTest = () => {
    const matchedTests = selectedSymptoms.map((symptom) => testSuggestions[symptom]).filter(Boolean);
    setRecommendedTest(matchedTests.length ? matchedTests : ["Aucun test recommandé"]);
  };

  return (
    <ScrollView style={{ padding: 20 }}>
      <Text style={{ fontSize: 20, fontWeight: "bold" }}>Sélectionnez vos symptômes :</Text>
      {symptomsList.map((symptom) => (
        <TouchableOpacity
          key={symptom}
          onPress={() => toggleSymptom(symptom)}
          style={{
            padding: 10,
            marginVertical: 5,
            backgroundColor: selectedSymptoms.includes(symptom) ? "#4CAF50" : "#ddd",
            borderRadius: 5,
          }}
        >
          <Text>{symptom}</Text>
        </TouchableOpacity>
      ))}

      <Button title="Trouver un test" onPress={findTest} />

      {recommendedTest.length > 0 && (
        <View style={{ marginTop: 20 }}>
          <Text style={{ fontSize: 18, fontWeight: "bold" }}>Tests recommandés :</Text>
          {recommendedTest.map((test, index) => {
            const kitName = test.split(" (")[0];
            return (
              <Text key={index} style={{
                backgroundColor: kitColors[kitName] || "#bdc3c7",
                padding: 5,
                marginTop: 5,
                borderRadius: 5,
                color: "#fff"
              }}>
                {test}
              </Text>
            );
          })}
        </View>
      )}
    </ScrollView>
  );
}
