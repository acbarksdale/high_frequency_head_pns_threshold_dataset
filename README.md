# High-Frequency PNS Threshold Dataset

This repository contains high-frequency PNS (Peripheral Nerve Stimulation) threshold measurements in the human head. The dataset was acquired at the Athinoula A. Martinos Center for Biomedical Imaging.

## 📄 Dataset Description

### Excel File

We provide an Excel (`.xlsx`) file titled:
high_freq_pns_deidentified_subject_data.xlsx


This file contains **de-identified subject data** with the following fields:

- Head circumference (cm)
- Age (years)
- Subject-reported gender
- Height (inches)
- Weight (lbs)
- BMI

An additional sheet in the same file contains **processed subject thresholds versus frequency**. The raw data folder corresponding to this processed data can be found below.

---

### Raw Data Folder

All raw data—including current/voltage waveforms and subject titration responses—can be found at the following dropbox link: https://www.dropbox.com/scl/fo/6dik1n42jhnrczi9mi5kt/ABVo9UGYQbPaUyNstIToX3E?rlkey=9dhntga0b04ynf59ur0dl3wpz&dl=0. We will upload the data directly to this repository.

Inside this folder, each subject has a subfolder titled in the format: [Subject ID No.]_[Date]_frequency_sweep

**Example:**
294166_Nov-21-2023_frequency_sweep/


Inside each subject folder, `.pkl` files contain raw data stored as Python dictionaries.

#### Example `.pkl` File: 21-11-2023_16h-24m-24s_frequency_sweep_200Hz_256cycles_tau_25cycles.pkl

This file corresponds to the 200 Hz titration for Subject 294166.

---

The dictionaries stored in the .pkl file each have the following key structure:

'subject_id': subject ID, (ex: 294166 - the ID corresponding to Subject No. ca be found in 'high_freq_pns_deidentified_subject_data.xlsx')

'experiment_date': string containing the experiment date

'timestamp': string containing the timestamp at the beginning of the titration, in format dd-mm-yyyy_hh_mm_ss

'experiment_type': this is always a string 'frequency_sweep'

'resonant_frequency': prior to each subject, we performed a resonant characterization and recorded the measured resonant frequency here, in Hz

'impedance_at_resonance': we further record the measured impedance at resonance in Ohms

'num_cycles_per_pulse': for our experiment, we always use 256 cycles per pulse in this field

'titration_responses': a list of bools containing the processed titration response (either True or False - the raw response trace can be found in 'trace_data' below. The index of each response corresponds to the same index in the values corresponding to the 'applied_voltages', 'applied_currents', 'measured_tau_cycles', 'trace_data', 'applied_daq_voltages', 'measured_impedances' keys.

'applied_voltages': a list of the processed voltage amplitude of the voltage trace measured across the LCR stimulating circuit, in Volts

'applied_currents': a list of the processed current amplitude of the current trace measured through the LCR stimulating circuit, in Amps

'measured_tau_cycles': a list of the measured tau (rampup time constant in number of cycles) for each applied pulse

'trace_data': a list the raw oscilloscope traces collected for each pulse. Each element of the list is a dictionary. The keys included are:

'imon_trace': the raw oscilloscope trace corresponding to the current measurement

'vmon_trace': the raw oscilloscope trace corresponding to the voltage measurement

'tx_trace': unused

'imon_fs': the sampling rate of the current oscilloscope trace

'vmon_fs': the sampling rate of the voltage oscilloscope trace

'tx_fs': unused

'response_data': the raw oscilloscope trace corresponding to the subject reponse (high if the push button was pressed, low otherwise)

'applied_daq_voltages': a list of applied daq voltages as input to the amplier

'measured_impedances': a list of impedances for each pulse, calculated as the ratio of the applied_voltage / applied_current

'computed_current_threshold': the computed PNS threshold (in A) - this was just used at runtime for a quick metric

'computed_threshold_width': the computed PNS threshold width (in A) - again this was just used at runtime

'amplifier': a string containing which amplifier was used to drive this particular configuration

'desired_tau_cycles': the desired rampup time constant (in number of cycles)


## 📦 Loading the `.pkl` Data

You can load a `.pkl` file in Python using:

```python
import pickle

def load_pickle(filename):
    """
    Loads .pkl file and returns the dictionary.
    """
    with open(filename, 'rb') as handle:
        data = pickle.load(handle)
    return data










