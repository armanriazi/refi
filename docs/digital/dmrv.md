---
keywords:   armanriazi ArmanRiazi ReFi Digital dMRV reporting  monitoring  verificatio MRV
author: "Arman Riazi"
search:
  exclude: false
type:  MRV
feature:  MRV dMRV
author: "Arman Riazi"
title: "ReFi Digital MRV"
show:
  - feedback
---

# Credit issuance

**Credit issuance is ultimately a process of (MRV)** monitoring, reporting, and verification: 
monitoring to calculate a project’s emissions reductions (i.e., credits), reporting to document the calculations before a credit registry, and verification of the calculations by independent auditors. Credit registries, such as **Verra**, now refer to future digital crediting platforms as digital MRV (DMRV) platforms. Since our founding in 2018, Pachama has sought to lead the way in DMRV and become the first registry-certified DMRV platform [29].

DMRV platforms would modernize credit issuance in at least 4 critical ways [29].

1. Shift from manual data collection **to satellite observation** [29].
2. Shift from inconsistent, manual calculation to uniform, **automated calculation** [29].
3. Shift from auditing projects to **certifying platforms** [29].
4. Shift from static documents to interactive, digital **visualization** [29].

# A Core Crediting Equation

[[carbon_credit]]
To calculate each term in this equation, a DMRV must produce three core outputs [29]: 
- [x] an estimate of baseline emissions
- [x] a map of forest carbon
- [x] a map of forest cover change (deforestation and/or reforestation) to know where carbon has been lost or gained

<figure markdown>
![dMRV](https://armanriazi.github.io/refi/assets/attachments/dmrv.png){ width=800 height=600 align=center }
<figcaption>DMRV Platform [29].</figcaption>
</figure>


# Key characteristics of DMRV 

That distinguish it from manual MRV include [31]:

- [x] Digitally native platforms that can **receive data from a range of sensors and data sources, including manual inputs, and automatically analyze and process data using algorithms and advanced models**.
- [x] **Checkpoints** and checklists within the platforms that ensure **consistency and accuracy**.
- [x] **A platform** that can be periodically certified for use within specified project parameters This allows some **project validation and verification** procedures to be automated to streamline project development while ensuring integrity.

# Climate Connect

<figure markdown>
![Biomass estimation](https://armanriazi.github.io/refi/assets/attachments/mustafa-biomass.png){ width=600 height=400 align=center }
<figcaption>Biomass estimation [32].</figcaption>
</figure>

CCD has developed ML-based models to estimate canopy height and above-ground biomass (AGB) over woodlands at sub-national scale. This allows for identification of forest deforestation and disturbance, and aids efforts to conserve biodiversity hotspots. How it works:

## Remote sensing

Our ML models use high-granularity optical and radar satellite data from the ESA and NASA (1984-present). This allows for high precision remote sensing around the globe, with a minimum accuracy of 80% for forest cover, and 70% for biomass and canopy height [32]

## Geospatial data

The ML models share processed and analysed data via web-GIS portals. The data contains 2D/3D geospatial visualisation, including variables which correlate to geographical coordinates. This can reveal temporal and spatial patterns [32].

## Community data collection

Digital data collection devices and tools measure social impact indicators [32].

## Forest fire detection and monitoring

CCD utilises 3D imaging data from the following sources to deliver its active forest fire detection and monitoring services [32]:

- [x] MOSDAC: Meteorological and Oceanographic Satellite Data Archival Centre
- [x] MODIS: Moderate Resolution Imaging Spectroradiometer
- [x] VIIRS: Visible Infrared Imaging Radiometer Suite

<figure markdown>
![Forest fire detection and monitoring](https://armanriazi.github.io/refi/assets/attachments/mustafa-2.png){ width=600 height=400 align=center }
<figcaption>Forest fire detection and monitoring [32].</figcaption>
</figure>

## DMRV for renewables

CCD offers forecasting and scheduling services for independent power producers [32].

### Data sources

SCADA systems or other third party sources; India’s Central Transmission Utility (CTU); daily generation reports (DGRs) [32].

### Data collection

SCADA data is compared with other sources to understand the issuance of carbon credits. Real-time data is also shared daily with the Global Carbon Council (GCC), for both energy and carbon credits [32].

### Data auditing

SCADA data is audited against other sources to identify discrepancies with meter data. This audit provides an optimised estimation of the issued carbon credits through SCADA data [32].

Having an **independent source of data (such as SCADA) to verify the amount of carbon credits** being claimed by projects on a registry can significantly reduce the costs associated with verification. Another major benefit is that this can accelerate the issuance of carbon credits, allowing new issuances on a bi-annual or monthly basis rather than once a year [32].

### Automated SDG extraction from project documents

Identifying which SDGs a project contributes to is a crucial task, and one that can be very time consuming for companies or project developers who rely solely on manual methods. However, AI can be used to intelligently extract SDGs relevant to a particular project based on the unstructured content contained in related documents (e.g. images and text). This method relies on Natural Language Processing (NLP) [32].

CCD has conducted R&D and testing on more than 50 documents in order to develop its own NLP service. Currently, the company’s models operate with 85% accuracy, which is projected to reach 95% within the next two quarters. CCD utilises the following **AI models for SDG extraction** from project documents [32]:

- [x] GPT2
- [x] All-mpnet-base-v2
- [x] All-distilroberta-v1
- [x] miniLM-L12-v2
