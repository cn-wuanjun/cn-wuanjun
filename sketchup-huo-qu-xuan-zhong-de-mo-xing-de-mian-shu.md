# Sketchup 获取选中的模型的面数

ruby脚本代码

```ruby
$arr_entityID = Array.new
$face_count = 0

def dealFaceCount (_entity)
    if _entity.is_a? Sketchup::Face
        unless $arr_entityID.include?(_entity.entityID)
            $arr_entityID.push(_entity.entityID)
            $arr_entityID.uniq!
        end
    elsif (_entity.is_a? Sketchup::Group)
        if _entity.entities.size > 0
            _entity.entities.each { |_entity_child|
                dealFaceCount(_entity_child)
            }
        end
    elsif _entity.is_a? Sketchup::Edge
        # faces = _entity.faces
        # $face_count = $face_count + faces.size
    elsif _entity.is_a? Sketchup::ComponentInstance
        _entities = _entity.definition.entities
        _entities.each { |_entity_child1|
            dealFaceCount(_entity_child1)
        }
    else
        # $arr_other_typename.push(_entity.typename)
    end
    return
end

selection.each { |entity|
    dealFaceCount(entity)
}
$face_count = $arr_entityID.size
```
