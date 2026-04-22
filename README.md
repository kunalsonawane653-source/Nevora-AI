# Nevora-AIfrom fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Vitals(BaseModel):
    hr: int
    spo2: int
    bp: int

def amma_analysis(hr, spo2, bp):
    alerts = []

    if spo2 < 90:
        alerts.append("Critical: Low Oxygen")

    if hr > 120 or hr < 50:
        alerts.append("Warning: Heart Rate Abnormal")

    if bp > 180 or bp < 90:
        alerts.append("Emergency: BP Issue")

    if not alerts:
        alerts.append("Stable")

    return alerts

@app.post("/analyze")
def analyze(vitals: Vitals):
    result = amma_analysis(vitals.hr, vitals.spo2, vitals.bp)
    return {"alerts": result}
