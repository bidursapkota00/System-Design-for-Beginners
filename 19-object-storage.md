# 19 - Object Storage

## Overview

Object storage is a data storage architecture that manages data as discrete units called objects, rather than as files in a hierarchical file system or blocks in a block storage system. Objects are stored in a flat address space, making object storage highly scalable and well-suited for unstructured data.

## Key Characteristics

### Object Structure

- **Data**: The actual content being stored (document, image, video, etc.)
- **Metadata**: Descriptive information about the object (creation date, size, custom attributes)
- **Unique Identifier**: Globally unique ID or key used to retrieve the object

### Architecture

- **Flat Namespace**: No hierarchical directory structure. uses flat namespace with globally unique identifiers
- **RESTful APIs**: Typically accessed via HTTP-based APIs (GET, PUT, DELETE)
- **Web-Scale Design**: Built for massive scalability (petabytes and beyond)
- **Distributed**: Data distributed across multiple nodes and often multiple locations

## Advantages

- **Unlimited Scalability**: Can scale horizontally to exabytes of data
- **Cost-Effectiveness**: Often less expensive than block or file storage for large datasets
- **Data Durability**: Multiple copies of data stored across different devices/locations
- **Rich Metadata**: Extended object information beyond basic file attributes
- **Global Access**: Objects accessible from anywhere via HTTP(S)
- **Immutability**: Objects typically aren't modified but replaced entirely

## Limitations

- **Performance**: Generally higher latency than block storage
- **Limited File System Operations**: No direct file locking or appending
- **Not Suitable for Databases**: Poor fit for transactional workloads
- **Limited OS Integration**: Not directly mountable as a traditional file system

## Use Cases

- **Static Content**: Websites, images, videos, documents
- **Backup and Archive**: Long-term retention of data
- **Data Lakes**: Storage for big data analytics
- **Cloud-Native Applications**: Microservices and containerized applications
- **IoT Data Storage**: Sensor data and device telemetry
- **Media and Entertainment**: Large media file storage

## Storage Classes & Tiering

- **Hot Storage**: Frequently accessed data with low latency
- **Cool Storage**: Infrequently accessed data with slightly higher latency
- **Cold Storage**: Rarely accessed data with higher retrieval times
- **Archive Storage**: Long-term preservation with highest retrieval times
- **Auto-tiering**: Automatic movement between tiers based on access patterns

## Popular Object Storage Implementations

### Public Cloud

- **Amazon S3** (Simple Storage Service)
- **Google Cloud Storage**
- **Microsoft Azure Blob Storage**
- **IBM Cloud Object Storage**

## Object Storage vs. Other Storage Types

### Object vs. File Storage

- File storage uses hierarchical directories while object storage uses flat namespace
- File storage offers POSIX compliance while object storage offers custom metadata
- File storage often has lower latency for small files

### Object vs. Block Storage

- Block storage stores data in fixed-sized blocks while object storage uses variable-sized objects
- Block storage offers better performance for databases and transactions
- Object storage provides better scalability for large unstructured datasets

## Blob Storage

Blob (Binary Large Object) storage is a subset of object storage optimized for storing large unstructured data.

### Key Features

- **Unstructured Data**: Handles any binary data without enforcing structure
- **Tiered Storage**: Often offers hot, cool, and archive access tiers
- **CDN Integration**: Commonly integrated with Content Delivery Networks
- **Public/Private Access**: Configurable access controls and public URLs

### Storage Tiers

- **Hot Tier**: Frequent access, higher storage cost, lower access cost
- **Cool Tier**: Infrequent access, lower storage cost, higher access cost
- **Archive Tier**: Rarely accessed data, lowest storage cost, highest retrieval cost
