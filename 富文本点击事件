# gist
- (void)configureCell:(VisaKeyValueObject *)detail
{
    _titleLabel.text = detail.visaTitle;
    _textView.text = [detail.visaContent stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
    _textView.delegate = self;
    _contentString = detail.visaContent;
    
    NSMutableAttributedString *attributedString = [[NSMutableAttributedString alloc] initWithString:_textView.text
                                                                                         attributes:@{NSFontAttributeName:font_Info,
                                                                                                      NSForegroundColorAttributeName:color_Secondary}];
    NSMutableParagraphStyle *paragraphStyle = [[NSMutableParagraphStyle alloc] init];
    [paragraphStyle setLineSpacing:2.5];
    [attributedString addAttribute:NSParagraphStyleAttributeName value:paragraphStyle range:NSMakeRange(0, _textView.text.length)];
    
    CGFloat height = [attributedString boundingRectWithSize:CGSizeMake(SCREEN_WIDTH - 30, CGFLOAT_MAX)
                                                    options:NSStringDrawingUsesLineFragmentOrigin
                                                    context:nil].size.height;
    
    NSUInteger numberOfLines = ceilf(height/(font_Info.lineHeight + 2.5));
    
    if (numberOfLines > kMaxNumberOfLines) {
        _textViewHeight.constant = (font_Info.lineHeight + 2.6) * kMaxNumberOfLines;
    } else {
        _textViewHeight.constant = height+1;
    }

    _textView.textContainer.maximumNumberOfLines = kMaxNumberOfLines;
    
    _textView.linkTextAttributes = @{NSForegroundColorAttributeName:UIColorFromRGB(0x00a0ff)};

    
    if (detail.visaHintUrlList.count > 0) {
        for (VisaHintUrlObject *url in detail.visaHintUrlList) {
            [attributedString addAttribute:NSLinkAttributeName value:url.visaHintUrl range:[_textView.text rangeOfString:url.visaHintText]];
        }
    }
    
    _fullAttributedString = [attributedString mutableCopy];
    
    _textView.attributedText = attributedString;
    
    __block NSUInteger i = 0;
    __block NSRange range = NSMakeRange(0, 0);
    __block CGFloat width = 0;
    if (numberOfLines <= kMaxNumberOfLines) {
        return;
    }
    
    CGRect frame = _textView.frame;
    frame.size.height = _textViewHeight.constant;
    _textView.frame = frame;
    
    [_textView.layoutManager enumerateLineFragmentsForGlyphRange:NSMakeRange(0, _textView.text.length) usingBlock:^(CGRect rect, CGRect usedRect, NSTextContainer *textContainer, NSRange glyphRange, BOOL *stop) {
        
        i++;
        
        if (i == kMaxNumberOfLines) {
            range = glyphRange;
            width = CGRectGetWidth(usedRect);
        }
    }];
    
    if ((i == kMaxNumberOfLines) || (range.length == 0 && i <= kMaxNumberOfLines + 1)) {
        return;
    }
    
    if (width + 84 < CGRectGetWidth(self.bounds) - 20) {
        [attributedString removeAttribute:NSParagraphStyleAttributeName range:range];
        [attributedString deleteCharactersInRange:NSMakeRange(range.location+range.length-1, _textView.text.length - range.location - range.length+1)];
        [attributedString appendAttributedString:[[NSMutableAttributedString alloc] initWithString:@"...查看更多 "
                                                                                        attributes:@{NSFontAttributeName:font_Info,
                                                                                                     NSForegroundColorAttributeName:color_Secondary}]];
    } else {
        
        NSUInteger numberOfDelete = 7 - floorf((CGRectGetWidth(self.bounds) - width - 20) / 14);
        [attributedString deleteCharactersInRange:NSMakeRange(range.length+range.location-numberOfDelete, _textView.text.length-range.length-range.location+numberOfDelete)];
        
        [attributedString appendAttributedString:[[NSMutableAttributedString alloc] initWithString:@"...查看更多 "
                                                                                        attributes:@{NSFontAttributeName:font_Info,
                                                                                                     NSForegroundColorAttributeName:color_Secondary}]];
    }
    
    _textView.attributedText = attributedString;
    
    
    [attributedString addAttribute:NSLinkAttributeName value:@"readMore" range:NSMakeRange(_textView.attributedText.length-5, 5)];
    
    _textView.attributedText = attributedString;
}
