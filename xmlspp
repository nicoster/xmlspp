#!/usr/bin/env luajit

local expat = require'expat'

local indent = -1, prev

function tab2xml(name, t)
	t = t or {}
	
	local ret = {}
	for k, v in pairs(t) do
		table.insert(ret,  k .."='\27[32m" .. v .. "\27[0m'")
	end
	if #ret > 0 then table.insert(ret, 1, '') end
	
	return '<\27[1;34m' .. name .. '\27[0m' .. table.concat(ret, ' ') .. '>'
end


local callbacks = setmetatable({}, {__index = function(t,k) return function(name, t) 
	
	if k == 'start_tag' then
		indent = indent + 1
		io.write('\n' .. string.rep('\t', indent) .. tab2xml(name, t))
		prev = name
	end

	if k == 'end_tag' then
		if prev ~= name then 
			io.write('\n' ..string.rep('\t', indent))
		end
		io.write('</\27[1;34m' .. name .. '\27[0m>')
		indent = indent - 1
	end

	if k == 'cdata' and name ~= '\n' then
		io.write(name)
	end
		

	-- print(k, tab2xml(name, t))
end end})
expat.parse({path='/dev/stdin'}, callbacks)



