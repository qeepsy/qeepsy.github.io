# qeepsy.github.io
# Qeepsy Events: Project Documentation

## Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [MVP Features & Workflow](#mvp-features--workflow)
4. [User Guide](#user-guide)
5. [Technical Implementation](#technical-implementation)
6. [Security & Anti-Fraud](#security--anti-fraud)
7. [Use Cases](#use-cases)
8. [Development Roadmap](#development-roadmap)

## Project Overview

Qeepsy Events is a Web3 platform that revolutionizes event attendance verification by enabling organizers to mint verifiable, dynamic NFTs for event attendees. Built on the Sui blockchain with Walrus decentralized storage, Qeepsy creates tamper-proof attendance badges that serve as digital certificates of participation.

### Key Value Propositions
- **Verifiable Attendance**: Immutable proof of event participation on blockchain
- **Dynamic Personalization**: NFTs customized with attendee-specific metadata and visuals
- **Soulbound Technology**: Non-transferable NFTs that prevent resale while maintaining authenticity
- **Seamless Experience**: Wallet-less onboarding for Web3 newcomers
- **Decentralized Storage**: Permanent content hosting via Walrus

### Target Users
- **Event Organizers**: Conference hosts, meetup coordinators, hackathon organizers
- **Attendees**: Event participants seeking verifiable credentials
- **Stakeholders**: DAOs, communities, and organizations requiring membership verification

## System Architecture

### Core Components

#### Frontend Layer
- **Web Interface**: React-based platform for organizers and attendees
- **Admin Dashboard**: Event management, analytics, and collection viewing
- **Mobile App**: QR scanning and wallet-less NFT claiming

#### Backend Services
- **Core API**: Handles NFT metadata generation and blockchain interactions
- **Dynamic Image Engine**: Generates personalized NFT visuals based on attendee data
- **Verification Service**: Processes QR scans and location-based check-ins
- **Email Service**: Automated confirmation and reminder notifications

#### Blockchain Infrastructure
- **Sui Network**: Smart contract execution and NFT storage
- **Move Modules**: 
  - `event_registry.move`: Event registration and management
  - `attendance_minter.move`: Attendance validation and minting triggers
  - `soulbound_nft.move`: NFT locking and transfer restrictions

#### Storage Layer
- **Walrus Storage**: Decentralized hosting for NFT metadata and images
- **IPFS Integration**: Backup storage solution for content permanence

### Data Flow Architecture
```
Organizer → Event Creation → Smart Contract Deployment
↓
Attendee Registration → Metadata Collection → QR Generation
↓
Event Check-in → Verification → Dynamic NFT Minting
↓
Blockchain Storage → Walrus Hosting → User Wallet
```

## MVP Features & Workflow

### Phase 1: Core Functionality

#### For Organizers
1. **Event Setup**
   - Upload custom artwork for NFT base design
   - Define maximum NFT supply for the event
   - Configure attendee data fields (name, tech stack, skills)
   - Set rarity tiers based on skill categories

2. **Distribution Configuration**
   - Choose between wallet-required or wallet-less distribution
   - Generate unique sign-up links for pre-registration
   - Configure skill-based rarity assignments

3. **Event Management**
   - Real-time dashboard showing registration numbers
   - Check-in interface for on-site NFT minting
   - Collection view of all minted NFTs (view-only access)

#### For Attendees
1. **Pre-Registration**
   - Sign up via organizer-provided link
   - Enter personal metadata (name, skills, tech stack)
   - Optional wallet address submission
   - Receive confirmation and reminder emails

2. **Event Check-in**
   - **With Wallet**: Present name at check-in table → instant NFT minting
   - **Without Wallet**: Install mobile app → scan unique QR code → receive NFT

3. **Post-Event**
   - Access NFT in connected wallet or mobile app
   - View in public gallery
   - Permanent ownership (soulbound, non-transferable)

### Complete User Journey

#### Workflow Diagram
1. **Artwork Upload & Analysis**: Organizer uploads base artwork, system analyzes and stores attributes
2. **Supply Configuration**: Maximum NFT quantity set for event capacity
3. **Smart Contract Deployment**: Secure minting contract created automatically
4. **Registration Setup**: Sign-up links distributed with metadata collection forms
5. **Pre-Event Communication**: Automated email confirmations and event reminders
6. **Live Check-in**: Two-path verification system (wallet vs. wallet-less)
7. **Dynamic Minting**: Real-time NFT generation with personalized attributes
8. **Collection Management**: Organizer dashboard with mint analytics and view-only NFT access

## User Guide

### Getting Started as an Organizer

#### Step 1: Account Setup
- Connect Web3 wallet (MetaMask, Suiet, or supported Sui wallets)
- Complete organizer verification process

#### Step 2: Create Your Event
- **Basic Information**: Event name, date, location, description
- **Artwork Upload**: Base design for NFT collection (PNG/JPG, max 10MB)
- **Supply Limits**: Set maximum attendee capacity
- **Metadata Fields**: Choose what information to collect from attendees

#### Step 3: Configure Distribution
- **Registration Type**: Wallet-required vs. wallet-less options
- **Skill Categories**: Define tech stacks and assign rarity levels
- **Check-in Method**: QR-based, location-based, or manual verification

#### Step 4: Launch Registration
- Share generated sign-up links
- Monitor registrations via dashboard
- Prepare check-in setup for event day

### Getting Started as an Attendee

#### Registration Process
1. Click organizer's sign-up link
2. Fill out registration form with required information
3. Optionally connect wallet for direct NFT delivery
4. Receive email confirmation with event details

#### Event Day Check-in
- **With Wallet**: Present yourself at check-in table, provide name and tech stack
- **Without Wallet**: Download Qeepsy mobile app, scan QR code at venue
- Receive NFT immediately upon successful verification

## Technical Implementation

### Smart Contract Architecture

#### NFT Metadata Structure
```json
{
  "name": "Event Name - Attendee Name",
  "description": "Attendance certificate for [Event]",
  "image": "walrus://[content-hash]",
  "attributes": [
    {"trait_type": "Event", "value": "Event Name"},
    {"trait_type": "Date", "value": "YYYY-MM-DD"},
    {"trait_type": "Location", "value": "Venue Name"},
    {"trait_type": "Attendee", "value": "Attendee Name"},
    {"trait_type": "Tech Stack", "value": "Developer/Designer/etc"},
    {"trait_type": "Rarity", "value": "Common/Rare/Epic"},
    {"trait_type": "Timestamp", "value": "Unix timestamp"}
  ]
}
```

#### Move Module Functions
- **Event Registration**: `create_event(organizer, max_supply, metadata)`
- **Attendance Verification**: `verify_checkin(event_id, attendee_data, proof)`
- **NFT Minting**: `mint_attendance_nft(event_id, recipient, metadata)`
- **Soulbound Enforcement**: `transfer_locked(nft_id)` returns error

### Integration Points

#### Walrus Storage API
- **Upload Endpoint**: `/store` for NFT metadata and images
- **Content Addressing**: Permanent URLs via content hashing
- **Retrieval**: Public access via `walrus://[hash]` URLs

#### Email Service Integration
- **Confirmation Emails**: Automated upon successful registration
- **Event Reminders**: Scheduled 24 hours before event
- **NFT Delivery Notifications**: Sent after successful minting

## Security & Anti-Fraud

### Protection Mechanisms

#### Check-in Security
- **One-time QR Tokens**: Each scan generates new QR code to prevent duplication
- **Time-based Validation**: QR codes expire after 30 seconds
- **Geolocation Verification**: Optional GPS-based check-in for high-security events
- **Manual Override**: Organizer approval required for suspicious check-ins

#### Blockchain Security
- **Soulbound Implementation**: Transfer functions disabled at contract level
- **Admin Controls**: Only event creator can trigger mints for their event
- **Supply Limits**: Hard caps prevent over-minting
- **Immutable Records**: All attendance data permanently stored on-chain

### Privacy Considerations
- **Minimal Data Collection**: Only essential information required
- **Wallet Optional**: No forced Web3 onboarding
- **Data Encryption**: All sensitive information encrypted in transit and at rest

## Use Cases

### Primary Applications
- **Tech Conferences**: Developer skill verification and networking credentials
- **Web3 Hackathons**: Participation proof and team formation assistance
- **Educational Events**: Course completion certificates and skill demonstrations
- **Community Meetups**: Membership verification and engagement tracking
- **DAO Governance**: Voting weight based on event participation history

### Extended Applications
- **Corporate Training**: Employee skill development tracking
- **Professional Certifications**: Industry-recognized credential issuance
- **Loyalty Programs**: Reward systems for repeat attendees
- **Access Control**: Event-based permissions for exclusive communities

## Development Roadmap

### Completed Features ✅
- Dynamic image generation engine
- QR-based attendance verification system
- Walrus decentralized storage integration
- Soulbound NFT implementation on Sui
- Basic admin dashboard and analytics

### Current Development 🔄
- Mobile app for wallet-less claims
- Enhanced email notification system
- Advanced rarity tier configuration

### Upcoming Features 📋
- **Badge Tiers & Gamification**: Multiple rarity levels with special benefits
- **API Access**: Developer endpoints for third-party integrations
- **Multi-event Collections**: Series-based NFT collections for recurring events
- **Social Features**: Attendee networking and collection sharing
- **Analytics Dashboard**: Advanced metrics and attendee insights

### Future Enhancements 🔮
- **Cross-chain Compatibility**: Expansion beyond Sui blockchain
- **AI-powered Personalization**: Machine learning for dynamic content generation
- **Marketplace Integration**: Optional trading for non-soulbound NFTs
- **Enterprise Solutions**: White-label platform for large organizations

---

## Support & Contact

For technical support, feature requests, or partnership inquiries, please contact our team through the official Qeepsy Events platform or visit our documentation portal for additional resources.