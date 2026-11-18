# Roller Coaster Designer

## Purpose

Create a simple educational simulator that helps students visualize how track shape affects speed and vertical g-force using conservation of energy and track curvature.

## Inputs

- Draggable track control points that set the track height in feet.
- Starting height: determined by the first control point; default `100 ft`.
- Gravity: fixed at `32.2 ft/s²`.
- A default track containing a starting hill, valley, and subsequent hills.
- Animation controls: Play, Pause, and Reset.
- Reset restores the default track and initial car position.
- Dragging a point immediately redraws the track and recalculates the ride.
- Friction is assumed to be zero.

## Calculations

Use feet for height and radius of curvature and use `g = 32.2 ft/s²`.

### Speed

Set initial height `h_initial` from the first control point. At each track position:

`v = sqrt(2g(h_initial - h))`

Convert feet per second to mph:

`mph = ft/s × 0.6818`

If `h > h_initial`, the point is unreachable. Do not calculate a square root of a negative number. Stop before or at the unreachable section and report a separate energy failure.

### Vertical g-force

Determine the local radius of curvature `R` from the surrounding track shape. Use a positive radius magnitude for the curved-track equations. The implementation must clearly define whether a point is classified as a valley or crest from the track slope and curvature.

- Valley: `n = 1 + v²/(gR)`
- Crest: `n = 1 - v²/(gR)`
- Flat track: curvature approaches zero, so g-force approaches `1 g`.

The ride fails energetically if the car cannot reach a higher point. The ride fails g-force-wise if `n > 4 g` or `n < -1 g`.

## Physics assumptions needing clarification

- The model does not include friction or drag.
- The track-shape curvature estimate is not yet specified; it must produce a radius in feet and identify crest/valley points consistently.
- Vertical g-force is the only g-force considered.
- Height is allowed to change continuously along the smoothed track.
- Exact speed and g-force at a control point depend on the chosen curvature calculation unless the track includes an explicit radius input.

## Outputs

- A prominently displayed interactive track with labeled height and horizontal-position scales.
- A visible animated car that starts at the first control point.
- Live speed in mph, track height in ft, and vertical g-force in g.
- A speed plot along the horizontal track position, with labeled axes and units.
- A g-force plot along the horizontal track position, with labeled axes and units.
- Distinct markers for:
  - Unreachable higher sections
  - g-force above `4 g`
  - g-force below `-1 g`
- A clear status of `Safe`, `Unsafe`, or `Cannot Reach Track`, with a short reason.

## Look and feel

- Track at the top and as the largest interface element.
- Live values and the status panel below or beside the track.
- Play, Pause, and Reset buttons with clear active states.
- Visible, draggable circular track points.
- Neutral background and grid lines, with a dark track line and a contrasting car.
- Green for safe, red for unsafe g-force, and amber for unreachable sections.
- Plot windows side-by-side on wide screens and stacked on smaller screens.
- Plots and track rescale with the viewport; no horizontal scrolling.
- On mobile, stack the track, live values, controls, status, and plots vertically.
- Avoid decorative features and unnecessary panels.

## Validation

Test with no friction and these values:

- Starting height: `100 ft`
- Valley height: `0 ft`
- Valley radius of curvature: `100 ft`
- Gravity: `32.2 ft/s²`

The expected values at the valley are approximately:

- Speed: `80.25 ft/s`
- Speed: `54.7 mph`
- Vertical g-force: `3.00 g`

Additional checks:

- Verify values when the car reaches the valley, not elsewhere on the track.
- Allow display rounding or a tolerance of about `±0.1`.
- Confirm the valley is classified `Safe` because `3 g` is between `-1 g` and `4 g`.
- Confirm that a smaller valley radius increases g-force and can trigger the unsafe state.
- Confirm that moving the first control point recalculates the starting height and ride.
- Confirm that the plots place the valley marker at the same horizontal position as the track.

## Out of scope

- Real-world structural design, operational safety, or passenger safety certification.
- Friction and drag.
- Lateral g-forces.
- Detailed vehicle or passenger dynamics.
- Three-dimensional track design.
- Real-world construction costs.