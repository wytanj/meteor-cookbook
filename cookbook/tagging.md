##  Tagging




````js
Template.taskItemTemplate.tag_objs = function () {
    var todo_id = this._id;
    return _.map(this.tags || [], function (tag) {
        return {todo_id: todo_id, tag: tag};
    });
};
Template.taskItemTemplate.adding_tag = function () {
    return Session.equals('editing_addtag', this._id);
};

Template.taskItemTemplate.events({
    'click .addtag': function (evt, tmpl) {
        Session.set('editing_addtag', this._id);
        Meteor.flush(); // update DOM before focus
        activateInput(tmpl.find("#edittag-input"));
    },
    'click .remove': function (evt) {
        var tag = this.tag;
        var id = this.todo_id;
        Todos.update({_id: id}, {$pull: {tags: tag}});
        Meteor.flush();
    }
});

Template.taskItemTemplate.events(okCancelEvents(
    '#edittag-input',
    {
        ok: function (value) {
            Todos.update(this._id, {$addToSet: {tags: value}});
            Session.set('editing_addtag', null);
        },
        cancel: function () {
            Session.set('editing_addtag', null);
        } 
}));
    
    
    
//-------------------------------------------------

Template.taskDetailCardTemplate.tag_objs = function(){
    try{
        return _.map(this.tags || [], function (tag) {
            return {todo_id: Session.get('selected_task_id'), tag: tag};
        });
    }catch(error){
        console.log(error);
    }
};
Template.taskDetailCardTemplate.adding_tag = function(){
    try{
        return false;
    }catch(error){
        console.log(error);
    }
};
````