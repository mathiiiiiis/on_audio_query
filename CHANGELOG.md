# Changelog

All notable changes to this project will be documented in this file.

## [2.9.3] - 2026-01-20

### Added
- **Artist separation system**: Automatic splitting of combined artist strings (e.g., "Artist A, Artist B") into individual artists
  - Centralized all artist splitting logic in `ArtistSeparatorConfig` for consistency
  - Pattern-based exception detection for band names with separators (scales infinitely, e.g., "X, The Y" pattern)
  - Regex-based multi-separator parsing (correctly handles "A, B & C feat. D")
  - Index-based lookup for querying songs by split artist (~100-1000x faster for large libraries)
  - LRU cache (50 entries) for frequently accessed split artists (~5000x faster on cache hits)
  - Support for 9 different separators: feat., ft., featuring, /, comma, &, and, x, X

### Technical Details
- **New file**: `ArtistSeparatorConfig.kt` - Centralized configuration for all artist separation logic
- **Modified**: `ArtistQuery.kt` - Builds split artist index and ID-to-name mappings during load
- **Modified**: `AudioFromQuery.kt` - Uses index-based lookup with LRU caching for split artist queries
- Pattern-based exceptions using regex: `^.+, the .+$`, `^.+ & the .+$`, `^.+ and the .+$`
- Performance: 20,000+ song libraries can query split artists in ~5ms (first access) and ~0.1ms (cached)
- Split artists use deterministic negative IDs to distinguish from MediaStore IDs

## [2.9.2] - 2026-01-19

### Changed
- **discNumber robustness**: Updated the `discNumber` getter to support both `String` and `int` values, automatically parsing `String` values to `int` when required
- Updated `on_audio_query_platform_interface` to v1.7.2
- Updated `on_audio_query_android` to v1.1.2
- Updated `on_audio_query_ios` to v1.1.2

### Technical Details
- Improved type handling in the Dart `SongModel.discNumber` getter to ensure consistent integer output across platforms

## [2.9.1] - 2026-01-19

### Added
- **discNumber support**: Added `discNumber` property to `SongModel` to expose disc number metadata from audio files
  - Android: Reads `MediaStore.Audio.Media.DISC_NUMBER` from MediaStore
  - iOS: Reads `discNumber` from `MPMediaItem`
  - Enables proper multi-disc album separation and display

### Changed
- Updated `on_audio_query_platform_interface` to v1.7.1
- Updated `on_audio_query_android` to v1.1.1
- Updated `on_audio_query_ios` to v1.1.1

### Technical Details
- Added `DISC_NUMBER` to Android cursor projection in `CursorProjection.kt`
- Added `disc_number` field to iOS `OnAudioHelper.swift`
- Added `discNumber` getter to Dart `SongModel` class

## [2.9.0] - Original version
- Base functionality from upstream repository