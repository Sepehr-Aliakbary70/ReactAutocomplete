/**
 * Created by S.Aliakbari on 6/21/2017.
 */
import React from 'react';
import onClickOutside from 'react-onclickoutside';
class Autocomplete extends React.Component {

    //Constructor of react component
    constructor(props) {
        super(props);
        this.notFoundMessage = `not found`;
        this.state = {
            options: this.props.items,
            selectedValue: typeof this.props.selectedValue != 'undefined' ? this.props.selectedValue : -1,
            showOption: false,
            visible: false,
            cursor: -1,
            notFound: false,
            mobile: false
        };
        this.handleClickOutside = this.handleClickOutside.bind(this);
    }
    //Handle click outside of component
    handleClickOutside() {
        this.setState({
            showOption: false,
        })
    }
    //React component lifecycle - Check if our special state change then autocomplete will be restart
    componentWillReceiveProps(nextProps) {
        this.setState({
            options: typeof nextProps.items != 'undefined' ? nextProps.items : this.state.options
        });
        if (nextProps.selectedValue != 'undefined') {
            this.setSelectedItem(nextProps.selectedValue);
        }
        if (nextProps.resetState != this.props.resetState) {
            this.setState({
                selectedValue: {Title: this.props.placeholder, Value: -1}
            });
        }
    }
    //React component lifecycle
    componentDidMount() {
        if (typeof this.props.selectedValue != 'undefined') {
            this.setState({
                selectedValue: this.props.selectedValue
            });
            this.setSelectedItem(this.props.selectedValue);
        }
        if (window.innerWidth <= 420) {
            this.setState({
                mobile: true
            })
        }
    }
    //Handle changes of input
    onInputChange(e) {
        this.setState({
            inputVal: e.target.value
        });
        if (typeof this.props.stateChange !== 'undefined') {
            let items = this.props.items;
            const filteredItems = [];
            if (items != null)
                items.map((item, index) => {
                    if (item.Title.toLowerCase().indexOf(e.target.value.toLowerCase()) >= 0) {
                        filteredItems.push(item);
                    }
                });
            this.setState({
                options: (filteredItems.length > 0) ? filteredItems : [{
                    Title: this.notFoundMessage,
                    Value: -1
                }],
                notFound: true,
                showOption: true,
                visible: true
            });
        }

    }
    //Set selected item
    setSelectedItem(id) {
        if (id == -1) {
            this.setState({
                selectedValue: {
                    Title: 'please select',
                    Value: -1
                }
            })
        }
        if (typeof this.props.items != 'undefined' && this.state.options != null) {
            this.state.options.map((item) => {
                if (item.Value == id) {
                    this.setState({
                        selectedValue: item
                    });
                }
            });
        }
    }
    //Handle drop down clicks
    onOptionClick(e) {
        let item = e.currentTarget.parentElement.parentElement.parentElement;
        let childItem = item.getElementsByClassName('input-val')[0];
        let value = childItem.value;
        if (value != '' || typeof value != 'undefined' || value != -1) {
            item.getElementsByClassName('input-container')[0].className = 'input-container';
        }
        if (e.target.id == -1) {
            this.setState({
                selectedValue: {
                    Title: 'please select',
                    Value: -1
                }
            })
        }
        else {
            this.setSelectedItem(e.target.id);
        }
        this.setState({
            showOption: false
        });
        if (this.props.changingUrl)
            this.props.urlChange(e.target.getAttribute('name'), e.target.id, 'add');

        if (this.props.changingState)
            this.props.stateChange(parseInt(e.target.id));

    }
    //Handle text input click
    onInputClick(e) {
        if (!this.state.showOption) {
            this.setState({
                showOption: true,
                visible: true
            })
        }
        else {
            this.setState({
                showOption: false,
                visible: false
            })
        }
    }
    //Popup disappearing
    hideAutocompleteHintPopup() {
        this.setState({
            showOption: false
        })
    }

    render() {
        const optionDOM = [];
        optionDOM.push(
            <div
                className={`input-dropdown-item`}
                id={'-1'} onClick={this.onOptionClick.bind(this)}
                key={`-1`} name={this.props.name}>
                please select
            </div>
        );
        let optionContainer;
        let self = this;
        if (typeof this.state.options != 'undefined' && this.state.options != null) {
            this.state.options.map((item, index) => {
                optionDOM.push(
                    <div
                        className={`input-dropdown-item ${this.state.cursor == index ? 'active' : ''}${this.state.selectedValue.Value == item.Value ? ' active' : ''}`}
                        id={item.Value} onClick={this.onOptionClick.bind(this)}
                        key={`${item.Value}`} name={this.props.name}>
                        {item.Title}
                    </div>)
            });
        }
        if (this.state.showOption) {
            optionContainer =
                <div
                    className={`input-dropdown-menu-with-hint${this.state.visible == true ? ' show' : ' hide'}${typeof this.props.disableItemPopup != 'undefined' ? ' disable-popup-on-mobile' : ' popup-on-mobile'} height-${typeof this.props.disableItemPopup == 'undefined' ? this.props.items.length > 5 ? '65' : (this.props.items.length + 1) * 10 : 'auto'}`}
                >
                    {typeof this.props.disableItemPopup == 'undefined' ? <div className="input-dropdown-header">
                        <svg onClick={this.hideAutocompleteHintPopup.bind(this)} className="icon icon-close"
                             dangerouslySetInnerHTML={{__html: '<use xlink:href="#icon-close"></use>'}}></svg>
                        <svg className="icon icon-homing"
                             dangerouslySetInnerHTML={{__html: '<use xlink:href="#icon-title"></use>'}}></svg>
                    </div> : ''}
                    <span className="autocomplete-icon-and-placeholder">
                {!this.props.disableAutoComplete ?
                    <input id={`${this.props.name}-hint`} onChange={this.onInputChange.bind(this)}
                           className="autocomplete-hint-input"
                           placeholder={this.props.placeholder}
                           type="text"/> : ''}
                        {!this.props.disableAutoComplete ? <svg className="icon"
                                                                dangerouslySetInnerHTML={{__html: '<use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#icon-tab-search"></use>'}}></svg> : ''}
                </span>
                    <div className={`input-dropdown-menu`}>
                        {
                            optionDOM
                        }
                    </div>
                </div>
        }
        return (
            <div ref={(autoComplete) => {
                this.autoComplete = autoComplete;
            }}
                 className={`input input-dropdown-container width-${typeof this.props.width != 'undefined' ? this.props.width : '1'}${this.props.fill ? ' fill' : ''}${this.props.class != undefined ? ` ${' ' + this.props.class}` : ''}${this.props.isRequired ? ' is-required ' : ''}${this.state.showOption ? ' opened' : ''}`}>
                <div className={`input-container`}>
                    <input
                        name={typeof this.props.name != 'undefined' ? this.props.name : ''}
                        type="text"
                        readOnly={true}
                        className={`input-val${this.props.disableAutoComplete ? ' disabled' : ''}`}
                        placeholder={this.props.placeholder}
                        onClick={this.onInputClick.bind(this)}
                        // onChange={this.onInputChange.bind(this)}
                        value={this.state.selectedValue != null ? this.state.selectedValue.Title : this.props.placeholder}
                        id={this.state.selectedValue != null ? this.state.selectedValue.Value : this.props.placeholder + '-1'}
                        autoComplete="off"
                    />
                    <svg className="icon icon-chevron-down"
                         dangerouslySetInnerHTML={{__html: '<use xlink:href="#icon-chevron-down"></use>'}}></svg>
                </div>
                {this.state.showOption ? <div className="mobile-autocomplete-background"></div> : ''}
                {
                    optionContainer
                }
            </div>
        )

    }
}
export default onClickOutside(Autocomplete)
