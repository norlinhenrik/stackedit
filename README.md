# stackedit

Plan as of June 24, 2025 for writing docs:

1. Write example.md with stackedit Use rst syntax for labels and references.
```
.. _my-label:

# Label demo

Reference to :ref:`my-label` depends on step 2.
```

2. Use pandoc to convert md to rst, with a lua-filter.
`pandoc example.md -o example.rst --lua-filter=md2rst.lua`
```
-- Preserve rst :ref: `my-reference` syntax
function Inlines(inlines)
  for i = 1, #inlines - 1 do
    if inlines[i].t == "Str" and inlines[i].text == ":ref:" and inlines[i+1].t == "Code" then
      -- Replace :ref: `my-reference` with RAW rst :ref:`my-reference`
      local  ref  =  ":ref:`" ..  inlines[i+1].text  ..  "`"
      inlines[i] =  pandoc.RawInline("rst", ref)
      inlines[i+1] =  pandoc.Str("") -- remove
    end
  end
  return inlines
end
```
3. example.rst will look like this:

```
.. \_my-label:

Label demo
==========

Reference to :ref:`my-label` depends on step 2.
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTE0NjU4MjFdfQ==
-->