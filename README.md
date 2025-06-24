# stackedit

Plan as of June 24, 2025 for writing docs:
1. Write with stackedit, and use rst syntax for labels and references.
2. Use pandoc to convert md to rst, with a lua-filter.
```
-- Preserve rst :ref: `my-reference` syntax
function Inlines(inlines)
  for i = 1, #inlines - 1 do
  if inlines[i].t == "Str" and inlines[i].text == ":ref:" and
  inlines[i+1].t == "Code" then
  -- Replace :ref: `my-reference` with RAW rst :ref:`my-reference`
  local  ref  =  ":ref:`" ..  inlines[i+1].text  ..  "`"
  inlines[i] =  pandoc.RawInline("rst", ref)
  inlines[i+1] =  pandoc.Str("") -- remove
  end
  end
  return inlines
end
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzMTM1ODE3M119
-->