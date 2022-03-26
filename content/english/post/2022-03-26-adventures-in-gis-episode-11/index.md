---
title: Adventures in GIS, Episode 11
author: 'Jeonghoon Lee'
summary: The eleventh episode to my trek through learning about GIS software
date: '2022-03-26'
slug: []
categories: []
tags: []
Description: ''
Tags: [ArcGIS, ArcMAP, Ryerson]
Categories: [ArcGIS, ArcMAP, Ryerson]
DisableComments: no
---

Our third report for CODG212 involved interpolating a dataset using the Ordinary Least Squares method, then investigating the dataset further for violations of statistical assumptions. I found that there was spatial autocorrelation and model misfit within the data upon evaluating the Koenker metric and Jarque-Bera probability, respectively. The Global Moran's I test also corroborated this observation. Using these findings, it was deemed appropriate to employ the Geographically Weighted Regression to interpolate data within the dataset. Upon comparing AIC values, it was found that the GWR did indeed have the better model fit.

Here is the copy of my report: https://thedatum-gis.s3.amazonaws.com/LeeJeonghoon_Lab3.pdf

I did particularly well with this week's report. It is certainly rewarding to see my progression in this course be positive.