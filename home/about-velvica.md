<!-- TITLE: About Velvica -->
<!-- SUBTITLE: A quick summary of About Velvica -->

# Velvica General Description (The Velvica Book)

## Introduction

The purpose of this document is to provide a general description of Velvica platform for internal use. 
A team member should always have ability to refresh basic knowledge about the platform and its general concept. 

The content of this document should contain information about the most permanent concepts and modules of the system. It should cause the stability of the document over time.
The two main scenarios of the document usage:
  
- A new team member introductory material.
- An existing team member knowledge refreshing material.
 

## About Velvica

Velvica is a cloud commerce platform that enables distributors and service providers to automate cloud services sales directly to end customers and through partners (resellers, affiliates). 

### Cloud commerce explanation

The most understandable analogue of cloud commerce platform is e-commerce platform ([Magento.com](http://magento.com), [Shopify.com](http://shopify.com)). I.e. a platform that enables any company to automate sales of physical goods to end customers in internet. But instead of physical goods Velvica automates cloud services sales such as cloud applications (SaaS), cloud infrastructure (IaaS) and cloud platforms (PaaS).

Cloud commerce platform is more complicated than regular e-commerce platform. In e-commerce physical goods case the purchase and delivery is the final steps of the process while in cloud services case it's only the beginning of end customer lifecycle process.

#### E-commerce life cycle

![E-commerce lifecycle](materials/e_commerce_lifecycle.png)

#### Cloud commerce life cycle

![Cloud commerce lifecycle](materials/cloud_commerce_lifecycle.png)

### General interaction scheme

The general Velvica scheme shows the interaction between the involved parties. Velvica integrates with cloud services and then automate sales of these cloud services to end customers directly and through resellers.

![General Velvica scheme](materials/general_velvica_scheme.png)

### Velvica's customers types

There are two common Velvica customers types that buy Velvica to automate cloud services sales:

- Distributor
- Service provider

#### Distributor

A distributor uses Velvica to sell third-party cloud services through resellers. Usually distributor doesn't have any its own services but there are no technical limitations to sell its own services alongside with third-party ones.

![Distributor Velvica scheme](materials/distributor_velvica_scheme.png)

#### Service provider

A service provider uses Velvica to sell its own service or number of its own services to end customers directly and through resellers. Also usually service provider sells a number of complementary and relevant third-party cloud services alongside with its own services.  

![Service provider Velvica scheme](materials/service_provider_velvica_scheme.png)

### Case studies

Here are the distributor and service provider implemented concepts on a real life examples:

- RentSoft (distributor)
- Lattelecom cloud (service provider)

#### RentSoft (distributor)

RentSoft ([rentsoft.ru](http://rentsoft.ru)) is a distributor that sells antivirus services such as Kaspersky, Eset, Dr.Web, etc. through Internet Service Providers (ISP). Initially Velvica platform was developed for RentSoft needs to automate antiviruses sales. But later on the team decides to spin off a Velvica company to focus on the platform development and sales.

On the one hand RentSoft has integration with antiviruses cloud platforms to be able to create and manage end customers account on antiviruses cloud platforms side. On the other hand RentSoft has integration with the billing systems of ISPs to be able to charge money from end customers accounts.

For each ISP RentSoft provides:

- White-label marketplace
- ISP's end customer's personal area subscription management widget

RentSoft also has its own marketplace [popodpiske.ru](http://popodpiske.ru) to sale antiviruses to end customers directly without ISP in the middle. Initially this website was developed for demo purpose. RentSoft demonstrated this website to prospective ISP as an example of white-label marketplace that RentSoft provides to each ISP.
  
![RentSoft Velvica scheme](materials/rentsoft_velvica_scheme.png)