# Notes for Superconductor Chain Task

## Initial Analysis

Goal: Identify the single best composition to synthesize from `data/not_synthesized.txt`, where "best" is defined as the fewest distinct elements among those whose Tc would be high (with ties broken by higher expected Tc).

Secondary goal: Report qualifying set Q1 - compositions containing Hg, exactly 4 distinct elements, with Tc >= threshold1 (where threshold1 = T* - 32, and T* is the maximum Tc within the homologous doping series of O6.080Ca1Cu2Ba2Hg1).

Let me start by measuring some compositions in the homologous series of O6.080Ca1Cu2Ba2Hg1 to determine T* (the maximum Tc in that series).

## Measuring the homologous series for O6.080Ca1Cu2Ba2Hg1

First, I'll check which compositions in the synthesized list belong to the homologous series of O6.080Ca1Cu2Ba2Hg1. The series consists of compositions with the same element set but varying oxygen content. So I need to find compositions with Ca, Cu, Ba, Hg, and O with varying amounts.

The series should be: O6.080Ca1Cu2Ba2Hg1, O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, etc.

Let me measure these compositions: