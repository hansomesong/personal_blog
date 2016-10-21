title: LTE-Basics
date: 2015-08-19 18:32:09
tags:
---

此贴是一个草稿性质的帖子，目的是记录下我对LTE的基础知识的理解和总结，若干时间后，此贴必将是一个宝贵的财富。

When a UE has packets to transmit, it performs random access (RA) during an allowable time slot. This timeslot is called an Access Grant Time Interval(ACGI) or RA opportunity (i.e., RA-slot).

The minimum resource scheduling unit for downlink and uplink transmission is referred to as an RB. One RB consists of 12 subcarriers (180 kHz) in the frequency domain and one subframe (1 ms) in the time domain. Details of the LTE-A physical layer is provided in [3GPP TS 36.300 V11.2.0, “Evolved Universal Terrestrial Radio Access (E-UTRA) and Evolved Universal Terrestrial Radio Access Network (EUTRAN), Overall Description,” June 2012., Sec. 5], and a brief review of M2M scheduling and signaling over LTE-A is provided in [4].
RA allows MTC devices to request a connection initialization. The RA-slots have bandwidth corresponding to six RBs (1.08 MHz) in the frequency domain, and the basic duration of an RA-slot is 1 ms in the time domain.

In LTE-Advanced networks, radio resources are usually divided into resource blocks (RBs) along the frequency domain per time slot, which is also referred to as subchannels in radio resource allocation. 

In the LTE-A, RA can be used for initial access to establish a radio link, resource request when no uplink radio resource has been allocated, scheduling request if no dedicated scheduling request has been configured (i.e., no dedicated physical uplink control channel [PUCCH] is available), and re-establishing a radio link after failure.

在物理层方面 \cite{KanZheng12}：
In order to meet the requirements of LTE- Advanced such as peak data rates of up to 1 Gb/s, more spectrum bands are needed. Besides the existing carriers for 3G networks, spectrum bands located at 450–470 MHz, 698–790 MHz, 2.3–2.4 GHz, and 3.4–3.6 GHz can be used for the deployment of LTE and LTE-Advanced networks. Moreover, LTE-Advanced has been defined to support scalable carrier bandwidth exceeding 20 MHz, potentially up to 100 MHz, in a variety of carriers for deployments.