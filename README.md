# BHOOMI
BHOOMI :- AI-powered predictive analytics system for early detection of land acquisition delays — SIH 2026, PS SIH26017

# BHOOMI

AI-powered predictive analytics for early detection of land acquisition delays in India.**

A Smart India Hackathon (SIH) 2026 software solution for identifying potential land acquisition delays early and supporting proactive, data-driven decision-making.
-----------------------------------------------------------------
| Project Details                     | Information             |
|-------------------------------------|-------------------------|
| Problem Statement ID                | SIH26017                |
| Team Name                           | INNOVANGERS             |
| Team ID                             | 169082                  |
| Theme                               | Smart Automation        |
| Category                            | Software                |
| Project Status                      | Architecture finalized; |
-----------------------------------------------------------------

## Problem Statement

Land acquisition is one of the most critical and time-sensitive stages of infrastructure development in India. When it gets delayed, it directly affects the execution of national and state-level projects, disrupting schedules and adding to the challenges of implementation.

These delays rarely come from a single cause. They usually stem from a mix of factors: administrative bottlenecks like prolonged approvals and poor coordination between departments, legal disputes and ownership conflicts, slow compensation disbursement, incomplete or inconsistent documentation, unresolved rehabilitation and resettlement obligations, and delays in communication across the departments involved.

The core problem identified in SIH Problem Statement SIH26017 is that no intelligent mechanism currently exists to identify which land acquisition projects are likely to face delays before those delays actually happen. Traditional monitoring and periodic reporting aren't enough to catch emerging risks early or help administrators act in time. Meanwhile, a lot of valuable information already exists in historical project records, timelines, compensation status, legal case data, and rehabilitation progress. It holds real predictive value, but it isn't being used that way today.

What the problem calls for, and what this project is built to deliver, is an AI-powered predictive analytics system that can flag at-risk projects early, generate a risk score for each one, and predict the probability of delay at every stage of the acquisition lifecycle. It should identify the specific factors driving a predicted delay and turn that into recommendations administrators can actually act on, all surfaced through dashboards, trend analysis, and GIS-based visualization, with automated alerts, role-based access, and continuous improvement as new data comes in.

The goal, simply put, is to shift land acquisition monitoring from reactive oversight to proactive, data-driven decision-making that leads to better planning, coordination, and faster infrastructure delivery.

## Our Solution (Overview)

Bhoomi is an AI-enabled decision support platform built to answer exactly what SIH26017 asks for: a way to know a land acquisition project is heading toward trouble before it actually stalls. Instead of waiting for a missed milestone to show up in a status report, the system watches every project continuously and tells administrators, in advance, where the risk is building.

Rather than giving a single yes or no answer on whether a project will be delayed, the system looks at each stage of the acquisition lifecycle on its own, from the initial notification through survey, compensation, possession, and rehabilitation and resettlement, and estimates the probability of delay at each one individually, using a survival analysis approach rather than a standard classifier. This matters because most real projects are still in progress at any given time, and a system that only outputs one flag for the whole project ends up throwing away exactly the information that makes early warning useful in the first place.

Every risk score the system produces comes with an explanation attached to it, not just a number. Alongside the prediction, the platform runs a separate check against actual legal deadlines under Indian land acquisition law, so a project is never flagged as risky on a model's opinion alone, it is checked against the real statutory timeline too. On top of this, the platform adds a verification layer most systems in this space leave out entirely, using free satellite imagery to independently check for signs of encroachment or unauthorized activity on a notified parcel, so the picture administrators see is not built on self-reported data alone.

The system is built entirely on open-source tooling, keeping it ready to deploy once it moves past the prototype stage.

The result is a platform that gives policymakers, district officers, legal teams, and citizens a live, explainable view of where every project stands, what is actually putting it at risk, and what the ground truth looks like independent of paperwork.

## Key Features

1. Bhoomi predicts delay risk separately for each of the five stages in the acquisition lifecycle, Notification, Survey, Compensation, Possession, and Rehabilitation and Resettlement, instead of giving one flat risk label for an entire project. This lets an administrator see exactly where in the process a project is likely to get stuck, not just that it might.

2. Every prediction comes with an explanation attached, built using SHAP, so the system shows the specific factors pushing a project's risk score up or down, things like pending approvals, slow compensation disbursement, or unresolved R&R obligations, rather than handing over a number with no reasoning behind it.

3. Running alongside the prediction model is a separate statutory compliance engine built directly from the RFCTLARR Act, 2013, which checks every project against its actual legal deadlines. A project can be flagged as at risk either because the model sees warning signs in the data or because it has genuinely missed a legal timeline, and the system always tells you which one it is.

4. The platform independently verifies ground conditions using free satellite imagery, comparing vegetation index readings over time on a notified land parcel to catch signs of encroachment or unauthorized construction, giving administrators a check that does not depend on anyone filing a report.

5. Document handling is built around an honest limitation rather than an overclaim. Printed government notices and applications are read automatically with strong accuracy, while handwritten citizen submissions are detected and routed to a human verification queue instead of being automatically transcribed, since reliable handwriting recognition for Indian regional scripts is not something current open-source tooling can genuinely promise.

6. Risk data is laid out visually through an interactive GIS dashboard, combining a point map of individual flagged projects with a district-level heat map showing where delays are concentrated, alongside trend charts and comparative analytics across regions and project types.

7. When a project's risk crosses a meaningful threshold, the system sends automated alerts over SMS, email, and push notifications so the right people find out without having to go looking for it.

8. Access to the platform is role based, built around the seven stakeholder groups involved in land acquisition, from central ministries down to the public citizen portal, with every action logged in a full audit trail for accountability.

9. The model keeps learning as new project data comes in, so its predictions improve over time instead of staying fixed to whatever it was trained on at the start.

## Why This Approach (Core Differentiators)

1. Stage-wise survival modeling instead of binary classification
   - Bhoomi predicts delay probability separately at each stage of the lifecycle, instead of forcing one yes or no label onto a project that is still in progress.

2. Explainability as a first-class output
   - Every risk score comes with a ranked breakdown of what is driving it, not just a number.

3. An independent statutory compliance layer
   - Every project is checked against actual RFCTLARR Act deadlines, so compliance is never left to the model's judgment alone.

4. Independent ground-truth verification
   - Free satellite imagery is used to verify real ground conditions, catching what administrative records alone might miss.

5. An honest, defensible scope
   - The system never claims a capability that current tooling cannot reliably deliver, such as automated handwriting recognition for Indian regional scripts.

6. A fully open-source, cloud-agnostic foundation
   - No proprietary licensing cost, and no dependency on any single cloud provider.
  
## System Architecture

```
+-------------------------------------------------------------------------------------------------------------------+
|                                            USER INTERFACE LAYER                                                   |
|  +-------------------------------------------------------------------------------------------------------------+  |
|  |  React SPA Framework                                                                                        |  |
|  |  +---------------------------+   +---------------------------+   +---------------------------------------+  |  |
|  |  | Leaflet / OpenLayers      |   | Apache Superset           |   | Role-Based Officer Dashboard          |  |  |
|  |  | (Interactive WebGIS Map)  |   | (Analytics & Reporting)   |   | (7 Stakeholder Roles, Audit Trail)    |  |  |
|  |  +---------------------------+   +---------------------------+   +---------------------------------------+  |  |
|  +-------------------------------------------------------------------------------------------------------------+  |
+---------------------------------------------------+---------------------------------------------------------------+
                                                    |
                                                    v  (REST / GeoJSON API Requests)
+---------------------------------------------------+---------------------------------------------------------------+
|                                            API & SERVING LAYER                                                    |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
|  | FastAPI Microservices                              |   | GeoServer                                          |  |
|  | (CRUD Services, Query Endpoints, GIS Gateway)      |   | (WMS / WFS Spatial Tile Rendering & Layer Serving) |  |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
+---------------------------------------------------+---------------------------------------------------------------+
                                                    |
                                                    v
+---------------------------------------------------+---------------------------------------------------------------+
|                                        BUSINESS LOGIC & WORKFLOWS                                                 |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
|  | Apache Airflow                                     |   | n8n Event Engine                                   |  |
|  | (ETL Orchestration & Scheduled Ingestion Pipelines) |   | (Automated Alerts, SMS / Email / Push)            |  |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
+---------------------------------------------------+---------------------------------------------------------------+
                                                    |
                                                    v
+---------------------------------------------------+---------------------------------------------------------------+
|                                            ANALYTICS & AI CORE                                                    |
|  +------------------------------------+  +-----------------------------------+  +------------------------------+  |
|  | Document Intelligence Pipeline     |  | Geospatial Change Engine          |  | Predictive Risk & Rules      |  |
|  | - LayoutParser (Handwriting Detect)|  | - Sentinel-2 Optical Data Ingest  |  | - XGBoost-AFT Survival Model |  |
|  | - Tesseract OCR (Printed Docs Only)|  | - Copernicus Data Space API       |  |   (Stage-wise Delay Probability)|
|  | - Human Verification Queue         |  | - NDVI Delta Computation (GDAL)   |  | - SHAP Engine (Attribution)  |  |
|  | (Handwritten Submissions)          |  |   (Encroachment Flags)            |  | - RFCTLARR Act Rules Engine  |  |
|  +------------------------------------+  +-----------------------------------+  +------------------------------+  |
+---------------------------------------------------+---------------------------------------------------------------+
                                                    |
                                                    v
+---------------------------------------------------+---------------------------------------------------------------+
|                                        DATA INGESTION & STORAGE LAYER                                             |
|  +-------------------------------------------------------------------------------------------------------------+  |
|  | PostgreSQL + PostGIS (Spatial Data Hub & Primary Relational Store)                                          |  |
|  |                                                                                                             |  |
|  |  [ Project Records ] <---> [ Compensation Data ] <---> [ Document Metadata ] <---> [ Satellite Raster ]     |  |
|  |                                                                                                             |  |
|  |  * RapidFuzz Entity Matching Engine (Name & Parcel ID Reconciliation Across Data Sources)                   |  |
|  +-------------------------------------------------------------------------------------------------------------+  |
+---------------------------------------------------+---------------------------------------------------------------+
                                                    |
                                                    v
+---------------------------------------------------+---------------------------------------------------------------+
|                                     CONTAINER & DEPLOYMENT LAYER                                                  |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
|  | Docker & Docker Compose                            |  | Deployment                                          |  |
|  | (Local Containerized Multi-Service Prototype Stack)|  | (Planned rollout once past prototype stage)         |  |
|  +----------------------------------------------------+   +----------------------------------------------------+  |
+-------------------------------------------------------------------------------------------------------------------+
```

## Tech Stack
| Layer                     | Technology                                                              | Purpose                                                    |
|---------------------------|-------------------------------------------------------------------------|------------------------------------------------------------|
| Remote Sensing            | [Sentinel-2](https://sentinel.esa.int/web/sentinel/missions/sentinel-2) | Free satellite imagery source for ground verification      |
| Remote Sensing            | [Copernicus Data Space API](https://dataspace.copernicus.eu/)           | Access and download of satellite imagery                   |
| Remote Sensing            | [GDAL](https://gdal.org/)                                               | Raster processing, NDVI computation                        |
| Remote Sensing            | [QGIS](https://qgis.org/)                                               | Spatial analysis and visualization during development      |
| Document Intelligence     | [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)             | Text extraction from printed documents                     |
| Document Intelligence     | [OpenCV](https://opencv.org/)                                           | Image preprocessing for document scans                     |
| Document Intelligence     | [LayoutParser](https://layout-parser.github.io/)                        | Detects handwritten content and routes it for human review |
| Document Intelligence     | [IndicBERT](https://huggingface.co/ai4bharat/indic-bert)                | Classification of citizen grievance text                   |
| Data Ingestion            | [FastAPI](https://fastapi.tiangolo.com/)                                | Backend API services                                       |
| Data Ingestion            | [Apache Airflow](https://airflow.apache.org/)                           | ETL orchestration and scheduled ingestion                  |
| Data Ingestion            | [RapidFuzz](https://github.com/rapidfuzz/RapidFuzz)                     | Entity matching across inconsistent records                |
| Spatial Database          | [PostgreSQL](https://www.postgresql.org/)                               | Primary relational data store                              |
| Spatial Database          | [PostGIS](https://postgis.net/)                                         | Spatial extension for geographic queries                   |
| Spatial Database          | [GeoServer](https://geoserver.org/)                                     | Serving map layers over WMS and WFS                        |
| Machine Learning          | [XGBoost](https://xgboost.readthedocs.io/) (AFT survival objective)     | Stage-wise delay probability prediction                    |
| Machine Learning          | [SHAP](https://shap.readthedocs.io/)                                    | Explainability and delay-driver attribution                |
| Machine Learning          | [scikit-learn](https://scikit-learn.org/)                               | Supporting preprocessing and evaluation                    |
| Machine Learning.         | RFCTLARR Rules Engine (custom-built)                                    | Statutory compliance checks against the RFCTLARR Act, 2013 |
| WebGIS and Dashboard      | [React](https://react.dev/)                                             | Frontend application framework                             |
| WebGIS and Dashboard      | [Leaflet](https://leafletjs.com/)                                       | Interactive WebGIS risk map                                |
| WebGIS and Dashboard      | [OpenLayers](https://openlayers.org/)                                   | Advanced map rendering and layer control                   |
| WebGIS and Dashboard      | [Apache Superset](https://superset.apache.org/)                         | Analytics dashboard and reporting                          |
| Automation and Deployment | [Docker](https://www.docker.com/)                                       | Containerized local deployment                             |
| Automation and Deployment | [n8n](https://n8n.io/)                                                  | Automated alerts over SMS, email, and push                 |
| Automation and Deployment | [NGINX](https://nginx.org/)                                             | Reverse proxy and request routing                          |


Every tool in this stack is free and open-source, keeping the project free of proprietary licensing costs and able to run on any infrastructure without architecture changes. The RFCTLARR Rules Engine has no external link since it is a component built specifically for this project, not an existing product.
