Developer: # Goal
Develop an initial routing solution for business outreach using a cleaned list of 9,500 businesses (attached, scraped from a library database). Most PO Boxes and FedEx drop-offs have been removed, but additional cleaning is required.

# Instructions
Begin with a concise checklist (3–7 conceptual bullets) outlining your approach (not implementation details) before starting the main workflow.

- Further clean the business list by:
  - Removing non-business addresses missed initially (e.g., PO Boxes, FedEx/UPS drop-offs, similar cases).
  - Identifying and removing duplicate or malformed records.
- For each business, obtain latitude and longitude. If missing, use geocoding based on the address. If unsuccessful, add the business to an error report with the reason.
- Plot all valid businesses on a map for visualization, providing both the map (as a downloadable file or image link) and underlying data (CSV or JSON) containing `business_name`, `address`, `latitude`, and `longitude`.
- Generate optimized circular outreach routes:
  - Routes must begin and end at **2101 N Gary Avenue, Wheaton, IL** (configurable—use the provided address as default; allow a custom input parameter).
  - Each route object must specify the actual start and end address.
- Assume a fixed 5-minute stop per business. No business should appear on more than one route.
- After each significant step (data cleaning, geocoding, routing), briefly validate the outcome (1–2 lines) and proceed or self-correct if validation fails. Summarize validations in the output.
- After completing major milestones, provide 1–3 sentence micro-updates summarizing progress and any blockers.

## Routing Options
1. **Option 1 – Fixed Number of Businesses Per Route:**
   - Use a Traveling Salesperson-type solver to create optimized circular routes.
   - Each route should include exactly 25 businesses.
   - For each route, report:
     - Ordered `sequence` of stops.
     - Total drive time.
     - Total stopped time (5 minutes per stop).
     - Combined total route time (in hours and minutes, rounded to the nearest minute).
     - Actual start and end address used.
     - In case of ties, select the route with the lowest combined total route time (drive + stopped time).

2. **Option 2 – Fixed Maximum Route Duration:**
   - Build optimized circular routes where total time (drive + stopped) does not exceed 3 hours (rounded to the nearest minute).
   - Each route includes as many businesses as possible within the constraint.
   - For each route, report:
     - Number of businesses.
     - Ordered sequence of stops.
     - Total drive time.
     - Total stopped time.
     - Combined total route time.
     - Actual start and end address used.
     - In case of ties, use route with lowest combined total route time.

- Any unroutable business (e.g., invalid/missing coordinates) should appear in an error report section, with details and reason.
- In all cases, prefer routes with minimized combined total route time (drive + stopped) in case of ties.
- Inform the user that the sum of all cleaned, unroutable, and routed businesses should equal the original input count and validate this in the output.

## Visualization
- Output downloadable links to route plots and provide the underlying plot data (CSV or JSON) for each route as well as for the overall business map. Plot data must include: `business_name`, `address`, `latitude`, `longitude` at minimum.

## Input Specification
- Accept a business list as CSV or JSON array, each entry with at least: `Business Name`, `Address`, `City`, `State`, `Zip`, and, where available, `Latitude`, `Longitude`. Geocode missing coordinates as needed.

## Output Format
Output must be a structured JSON using the exact format specified below. Ensure strict adherence to the following schema:

```
{
  "cleaned_businesses": [
    {
      "business_name": "...",
      "address": "...",
      "city": "...",
      "state": "...",
      "zip": "...",
      "latitude": 0.0,
      "longitude": 0.0
    }, ...
  ],
  "unroutable_businesses": [
    {
      "business_name": "...",
      "address": "...",
      "reason": "Could not geocode address"
    }, ...
  ],
  "routes_option_1": [
    {
      "sequence": [
        {
          "business_name": "...",
          "address": "...",
          "latitude": 0.0,
          "longitude": 0.0
        }, ...
      ],
      "start_address": "...",
      "end_address": "...",
      "total_drive_time_minutes": 0,
      "total_stopped_time_minutes": 0,
      "total_route_time_minutes": 0
    }, ...
  ],
  "routes_option_2": [
    {
      "sequence": [
        {
          "business_name": "...",
          "address": "...",
          "latitude": 0.0,
          "longitude": 0.0
        }, ...
      ],
      "number_of_businesses": 0,
      "start_address": "...",
      "end_address": "...",
      "total_drive_time_minutes": 0,
      "total_stopped_time_minutes": 0,
      "total_route_time_minutes": 0
    }, ...
  ],
  "plot": {
    "image_link": "...",
    "plot_data_file": "..." // File must be CSV or JSON with at least business_name, address, latitude, longitude columns/fields
  },
  "validation_summary": [
    {
      "step": "[cleaning | geocoding | routing]",
      "message": "..."
    }, ...
  ],
  "business_count_check": {
    "input_count": 0,
    "cleaned_count": 0,
    "unroutable_count": 0,
    "routed_count": 0,
    "match": true // True if input equals sum of cleaned, unroutable, and routed.
  }
}
```

## Output Details
- Each route object in `routes_option_1` and `routes_option_2` must include `start_address` and `end_address` fields.
- `plot_data_file` must be CSV or JSON and include at minimum: `business_name`, `address`, `latitude`, `longitude`.
- Include a `validation_summary` array with a short message after each major step (cleaning, geocoding, routing).
- Add a `business_count_check` object for input, cleaned, unroutable, and routed count, and a match validation.

Set reasoning_effort = medium based on the task complexity; make tool call outputs concise and final summaries more detailed. Attempt a first autonomous pass for each task unless critical information is missing; request clarification if success criteria cannot be met.
