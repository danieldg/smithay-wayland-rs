# The Subcompositor

The `wl_subcompositor` allow the client to assign the "subsurface" role to a `wl_surface` by
creating an associated `wl_subsurface` object.

## The Subsurfaces

When a surface is assigned the `wl_subsurface` role, it is associated to a parent surface. The
parent surface can be a subsurface of an other surface, and thus a tree of subsurface can be
created.

A tree of subsurfaces has the same visibility state as the surface at its root. This means the root
surface must have a role other than "subsurface" for the tree to be displayed[^1]. Regarding to display,
the compositor is expected to handle the whole tree of subsurfaces as if it was a single big surface:
the client can set the location of each subsurface relative to its parent (it is not required for it
to be within the borders of the parent).

Subsurfaces can typically be used for part of the UI that need to be updated by different means. For
example, a video player could have the video itself being a subsurface of the GUI surface: this way
it can update its contents with buffer coming directly from the (possibly hardware) video decoding
unit.

Subsurfaces can also be used to draw window decorations: this gives an easy way for the client to
know if the pointer is on the decorations or the main surface (see
[next chapter](./wayland/p_core/seat.html) for details about pointer input). But in general
subsurfaces should not be used for general UI composing: this is expected to be done client-side by
the GUI library.

--------

[^1] This means while, strictly speaking, even if a cycle of subsurface is possible to create, the
compositor will never attempt to display it. Thus creating such a cycle is both harmless and useless.