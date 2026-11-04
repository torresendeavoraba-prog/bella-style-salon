// OBJETOS (Requisito: Al menos 2 objetos)
const salonServices = {
    haircut: { name: "Hair Styling & Cut", price: 45 },
    coloring: { name: "Full Color & Highlights", price: 120 },
    facial: { name: "Rejuvenating Facial", price: 85 },
    manicure: { name: "Luxury Manicure", price: 35 }
};

const bookingState = {
    selectedServiceKey: "",
    clientEmail: "",
    isFormValid: false
};

// ARREGLOS (Requisito: Al menos 2 arreglos)
const serviceList = ["haircut", "coloring", "facial", "manicure"];
const validationErrors = [];

document.addEventListener("DOMContentLoaded", () => {
    loadStoredPreference();
    setupInteractiveFeature();
    setupFormValidation();
});

function setupInteractiveFeature() {
    const serviceSelector = document.getElementById("serviceSelector");
    const priceDisplay = document.getElementById("priceDisplay");

    if (serviceSelector && priceDisplay) {
        serviceSelector.addEventListener("change", (e) => {
            const chosenKey = e.target.value;
            bookingState.selectedServiceKey = chosenKey;

            if (salonServices[chosenKey]) {
                const serviceInfo = salonServices[chosenKey];
                priceDisplay.textContent = `Selected: ${serviceInfo.name} - $${serviceInfo.price}`;
                localStorage.setItem("preferredService", chosenKey);
            } else {
                priceDisplay.textContent = "Please select a service above.";
                localStorage.removeItem("preferredService");
            }
        });
    }
}

function loadStoredPreference() {
    const savedService = localStorage.getItem("preferredService");
    const serviceSelector = document.getElementById("serviceSelector");
    const priceDisplay = document.getElementById("priceDisplay");

    if (savedService && serviceSelector && priceDisplay && salonServices[savedService]) {
        serviceSelector.value = savedService;
        bookingState.selectedServiceKey = savedService;
        priceDisplay.textContent = `Restored Preference: ${salonServices[savedService].name} - $${salonServices[savedService].price}`;
    }
}

function setupFormValidation() {
    const bookingForm = document.getElementById("bookingForm");
    const emailInput = document.getElementById("clientEmail");
    const errorContainer = document.getElementById("emailError");

    if (bookingForm && emailInput) {
        bookingForm.addEventListener("submit", (event) => {
            validationErrors.length = 0;
            let isValid = true;
            const emailValue = emailInput.value.trim();
            
            if (emailValue === "") {
                validationErrors.push("Email address is required.");
                isValid = false;
            } else if (!emailValue.includes("@") || !emailValue.includes(".")) {
                validationErrors.push("Please enter a valid email address to proceed.");
                isValid = false;
            }

            if (!isValid) {
                event.preventDefault();
                if (errorContainer) {
                    errorContainer.textContent = validationErrors[0];
                }
            } else {
                if (errorContainer) {
                    errorContainer.textContent = "";
                }
            }
        });
    }
}
