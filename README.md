/**
 * listing-eleves-data.js — Chargement depuis Firebase
 */

document.addEventListener('DOMContentLoaded', async () => {
    const params    = new URLSearchParams(window.location.search);
    const className = params.get('classe');
    const trimestre = params.get('trimestre') || "Non spécifié";
    const dateVal   = params.get('date');

    if (!className || className === "null" || className === "-") {
        alert("Erreur : Aucune classe sélectionnée.");
        return;
    }

    // Titres
    if (document.getElementById('class-name'))  document.getElementById('class-name').textContent  = className;
    if (document.getElementById('term-name'))   document.getElementById('term-name').textContent   = trimestre;
    if (document.getElementById('date-info'))   document.getElementById('date-info').textContent   = dateVal || "-";

    const tbody = document.getElementById('listing-body');
    if (!tbody) return;
    tbody.innerHTML = '<tr><td colspan="9">Chargement des élèves...</td></tr>';

    try {
        // Charger depuis Firebase
        const savedConfig = localStorage.getItem("crcc_firebase_config");
        if (!savedConfig) throw new Error("Firebase non configuré");

        const { initializeApp } = await import("https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js");
        const { getFirestore, doc, getDoc } = await import("https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js");

        const app  = initializeApp(JSON.parse(savedConfig), "listing-app");
        const db   = getFirestore(app);
        const snap = await getDoc(doc(db, "config", "eleves"));

        tbody.innerHTML = "";

        if (snap.exists()) {
            const elevesData = snap.data();
            const eleves     = elevesData[className] || [];

            if (eleves.length === 0) {
                tbody.innerHTML = `<tr><td colspan="9" style="text-align:center;color:#aaa;padding:20px;">Aucun élève configuré pour la classe ${className}.</td></tr>`;
                return;
            }

            eleves.forEach(eleve => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="col-nom">${eleve.nom || ""}</td>
                    <td class="col-prenoms">${eleve.prenom || ""}</td>
                    <td class="col-observations" contenteditable="true" style="background-color:#fffdf0;cursor:text;"></td>
                    <td class="col-f" onclick="toggleCell(this)"></td>
                    <td class="col-c" onclick="toggleCell(this)"></td>
                    <td class="col-e" onclick="toggleCell(this)"></td>
                    <td class="col-at" onclick="toggleCell(this)"></td>
                    <td class="col-ac" onclick="toggleCell(this)"></td>
                    <td class="col-aa" onclick="toggleCell(this)"></td>
                `;
                tbody.appendChild(tr);
            });
        } else {
            // Générer 32 lignes vierges si pas d'élèves configurés
            for (let i = 0; i < 32; i++) {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="col-nom"></td>
                    <td class="col-prenoms"></td>
                    <td class="col-observations" contenteditable="true" style="background-color:#fffdf0;cursor:text;"></td>
                    <td class="col-f" onclick="toggleCell(this)"></td>
                    <td class="col-c" onclick="toggleCell(this)"></td>
                    <td class="col-e" onclick="toggleCell(this)"></td>
                    <td class="col-at" onclick="toggleCell(this)"></td>
                    <td class="col-ac" onclick="toggleCell(this)"></td>
                    <td class="col-aa" onclick="toggleCell(this)"></td>
                `;
                tbody.appendChild(tr);
            }
        }

    } catch (err) {
        console.error("Erreur chargement élèves:", err);
        tbody.innerHTML = '<tr><td colspan="9" style="text-align:center;color:#e74c3c;">Erreur de chargement. Vérifiez la configuration Firebase.</td></tr>';
    }
});

function toggleCell(cell) {
    if (cell.textContent === "X") {
        cell.textContent = "";
        cell.style.backgroundColor = "";
    } else {
        cell.textContent = "X";
        cell.style.fontWeight = "bold";
        cell.style.textAlign = "center";
    }
}
